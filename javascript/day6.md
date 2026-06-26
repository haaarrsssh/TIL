## Day 36 — Props Drilling & Lifting State Up

1. What Are They?

Props drilling is when you pass data down through multiple layers of components just to get it to a deeply nested child — even components in between that don't use the data themselves.
Lifting state up is the fix for sibling components needing to share data: move the state to their closest common parent, then pass it down via props.

Props drilling is the problem. Lifting state up is the solution pattern. Both are about one question: "where should this piece of state actually live?"

1. Why Does It Matter?

The moment your app has more than one component that needs the same data, you hit this immediately:

A search input in one component needs to filter a list in a sibling component
A shopping cart count needs to show in the header AND update from a product card
A theme toggle needs to affect the entire app, not just one button

Getting this wrong leads to duplicated state, bugs where one component doesn't know the other changed, and "prop drilling hell" through 5 layers of unused props. This is the #1 conceptual hurdle between "I can build one component" and "I can build a real app."

1. The 20% That Covers 80% of Real Work

The problem — props drilling

jsxfunction App() {
  const user = { name: "Harsh" };
  return <Dashboard user={user} />;
}

function Dashboard({ user }) {
  // Dashboard doesn't use "user" at all — just passes it through
  return <Sidebar user={user} />;
}

function Sidebar({ user }) {
  // Sidebar doesn't use it either — keeps passing it down
  return <Profile user={user} />;
}

function Profile({ user }) {
  // FINALLY used here, 3 levels deep
  return <p>Hello, {user.name}</p>;
}

Dashboard and Sidebar are just "pass-through" — they don't care about user, but they're forced to carry it. This is props drilling. Fine for 1–2 levels, painful beyond that.

The problem — siblings need to share state

jsx// ❌ This DOESN'T work — each component has its own isolated state
function SearchBox() {
  const [query, setQuery] = useState("");
  return <input value={query} onChange={(e) => setQuery(e.target.value)} />;
}

function ResultsList() {
  // No way to access SearchBox's "query" — they're disconnected
  return <p>Results would filter here...</p>;
}

function App() {
  return (
    <div>
      <SearchBox />
      <ResultsList />     {/*can't see the search query at all*/}
    </div>
  );
}

The fix — lift state up to the common parent

jsxfunction App() {
  const [query, setQuery] = useState("");   // state now lives in the PARENT

  return (
    <div>
      <SearchBox query={query} setQuery={setQuery} />
      <ResultsList query={query} />
    </div>
  );
}

function SearchBox({ query, setQuery }) {
  return (
    <input
      value={query}
      onChange={(e) => setQuery(e.target.value)}
    />
  );
}

function ResultsList({ query }) {
  const allItems = ["Apple", "Banana", "Cherry"];
  const filtered = allItems.filter((item) =>
    item.toLowerCase().includes(query.toLowerCase())
  );

  return (
    <ul>
      {filtered.map((item) => <li key={item}>{item}</li>)}
    </ul>
  );
}

Now both SearchBox and ResultsList are connected through App. Neither knows about the other directly — they both just talk to their shared parent's state.

The rule for deciding where state should live

1. Does only ONE component need this data?
   → Keep state local to that component.

2. Do TWO OR MORE components need this data (siblings, or a parent + child)?
   → Lift the state to their closest common parent.

3. Is the SAME data needed in many unrelated, far-apart components?
   → Consider Context API (next level up — not covered today).

children prop — an alternative to drilling

jsx// Instead of passing data through a wrapper that doesn't need it...
function Card({ children }) {
  return <div className="card">{children}</div>;
}

function App() {
  return (
    <Card>
      <Profile user={{ name: "Harsh" }} />   {/*Card never touches "user" at all*/}
    </Card>
  );
}

children lets you nest components directly without forcing the wrapper (Card) to know or care about props meant for something inside it.

1. Real-Life Mental Model

ConceptReal EquivalentProps drillingPassing a note through 4 people who don't need to read it, just to reach the 5thLifting state upPutting the shared notebook on the table everyone sits around, instead of in one person's pocketCommon parentThe shared table — everyone at it can read/write the same notebookchildren propHanding someone a box without caring what's inside it

The question to ask every time you add new state:

"Who actually needs to read or change this?" If the answer is "just me," keep it local. If the answer is "me and my sibling," lift it to our parent.

Key Takeaway

Props drilling isn't wrong — it's just painful past 2–3 levels. The fix isn't a special trick, it's a mindset: identify the closest common parent of everything that needs a piece of state, put the state there, and pass it down. This single pattern — "lift state to the nearest shared ancestor" — is the backbone of structuring any real React app before you ever need more advanced tools like Context or state libraries.
