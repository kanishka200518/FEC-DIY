SOLIDITY FEATURE:
1) Delete polls
Css:
.delete-button {
  background-color: #ff4d4d;
  color: white;
  padding: 8px 12px;
  border-radius: 5px;
  border: none;
  cursor: pointer;
  font-size: 0.9rem;
  transition: background-color 0.3s;
  margin-top: 10px;
}

.delete-button:hover {
  background-color: #cc0000;
}

React code:
const deletePoll = async (pollId) => {
  try {
    await contract.methods.deletePoll(pollId).send({ from: account });
    alert("Poll deleted successfully!");
    // Optionally refresh poll list here
  } catch (err) {
    console.error("Error deleting poll:", err);
    alert("Failed to delete poll.");
  }
};
{poll.creator.toLowerCase() === account.toLowerCase() && (
      <button className="delete-button" onClick={() => deletePoll(poll.id)}>
        Delete Poll
      </button>)

      Solidity:
  function deletePoll(uint _pollId) public {
    require(polls[_pollId].active, "Poll does not exist");
    // require(polls[_pollId].creator == msg.sender, "Only the creator can delete the poll");

    delete polls[_pollId];
    emit PollDeleted(_pollId);
}

event PollDeleted(uint pollId);
2) Preventing duplicate polls:

soliity:
mapping(string => bool) public existingQuestions;

require(!existingQuestions[_question], "Poll with this question already exists");
existingQuestions[_question] = true;

React code:
  const isDuplicate = polls.some(
      (poll) => poll.question.toLowerCase().trim() === newPollQuestion.toLowerCase().trim()
    );
    if (isDuplicate) {
      alert("⚠️ This poll question already exists (locally cached).");
      setLoading(false);
      return;
    }
     if (error?.message?.includes("Poll with this question already exists")) {
      alert("❌ A poll with this question already exists.");
    } else {
      alert("🚫 Failed to create poll.");
    }

Css:
.error-message {
  color: #dc3545;
  margin-top: 10px;
  font-weight: bold;
}

REACT FEATURE:

1)Light Dark mode toggle button:

css:


.App.dark {
  background-color: #121212!important;
  color: #f5f5f5 !important;
}

.App.dark .App-header {
  background-color: #333;
  color: #f5f5f5;
}

.App.dark .create-poll,
.App.dark .polls,
.App.dark .leaderboard {
  background-color: #1e1e1e;
  color: #f5f5f5;
}

.App.dark .poll-card {
  background-color: #2a2a2a;
}

.App.dark .leaderboard-table th {
  background-color: #333;
}

.App.dark .leaderboard-table tr:nth-child(even) {
  background-color: #1e1e1e;
}

.App.dark button,
.App.dark .vote-button {
  background-color: #444;
  color: #f5f5f5;
}
React code:

const [isDarkMode, setIsDarkMode] = useState(false);
const toggleTheme = () => {
  setIsDarkMode((prev) => !prev);
};
<div className={`App ${isDarkMode ? 'dark' : ''}`}>
     <button onClick={toggleTheme}>
    Switch to {isDarkMode ? 'Light' : 'Dark'} Mode
  </button>

  
2) Search bar to filter polls by keyword:
React code :
const [searchTerm, setSearchTerm] = useState("");
const filteredPolls = polls.filter((poll) =>
  poll.question.toLowerCase().includes(searchTerm.toLowerCase())
);
<div className="polls">
  

  <input
    type="text"
    placeholder="Search polls..."
    value={searchTerm}
    onChange={(e) => setSearchTerm(e.target.value)}
    className="search-input"
  />

  {filteredPolls.length > 0 ? (
    filteredPolls.map((poll) => (
      <div key={poll.id} className="poll-card">
        <h3>{poll.question}</h3>
        {/* rest of poll UI */}
      </div>
    ))
  ) : (
    <p>No polls found.</p>
  )}
</div>

 Css:

 .search-input {
  width: 100%;
  padding: 10px 12px;
  margin-bottom: 20px;
  border-radius: 5px;
  border: 1px solid #ccc;
  font-size: 1rem;
}






