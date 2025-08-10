# Let's create a basic MRI QUIZ web version as a zip file with HTML, CSS, and JS.

import zipfile
import os

# Create a temporary directory for the web files
base_dir = "/mnt/data/mri_quiz_web"
os.makedirs(base_dir, exist_ok=True)

# HTML content
html_content = """
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MRI QUIZ</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>MRI QUIZ</h1>
        <button id="toggle-mode">Toggle Light/Dark</button>
    </header>

    <main>
        <section id="quote-section">
            <blockquote id="quote">"Dream, dream, dream. Dreams transform into thoughts and thoughts result in action." – APJ Abdul Kalam</blockquote>
        </section>

        <section id="quiz-section">
            <div id="question-container">
                <p id="question">Question will appear here</p>
                <div id="answer-buttons"></div>
            </div>
            <button id="next-btn" style="display:none;">Next</button>
        </section>

        <section id="score-section">
            <h2>Your Score: <span id="score">0</span></h2>
        </section>
    </main>

    <script src="script.js"></script>
</body>
</html>
"""

# CSS content
css_content = """
body {
    font-family: Arial, sans-serif;
    background-color: white;
    color: black;
    margin: 0;
    padding: 0;
    text-align: center;
}

.dark-mode {
    background-color: #121212;
    color: white;
}

header {
    padding: 20px;
    background-color: #6200ea;
    color: white;
}

button {
    padding: 10px 15px;
    margin: 5px;
    cursor: pointer;
}

#quote {
    font-style: italic;
    margin: 20px;
}

#question-container {
    margin-top: 20px;
}
"""

# JavaScript content
js_content = """
const questions = [
    { question: "Who is known as the Missile Man of India?", answers: ["APJ Abdul Kalam", "Swami Vivekananda", "C V Raman"], correct: 0 },
    { question: "Swami Vivekananda attended which Parliament in 1893?", answers: ["World Parliament of Religions", "UN Assembly", "Indian National Congress"], correct: 0 }
];

let currentQuestionIndex = 0;
let score = 0;

const questionElement = document.getElementById("question");
const answerButtons = document.getElementById("answer-buttons");
const nextButton = document.getElementById("next-btn");
const scoreElement = document.getElementById("score");

function startQuiz() {
    currentQuestionIndex = 0;
    score = 0;
    showQuestion();
}

function showQuestion() {
    resetState();
    let currentQuestion = questions[currentQuestionIndex];
    questionElement.textContent = currentQuestion.question;

    currentQuestion.answers.forEach((answer, index) => {
        const button = document.createElement("button");
        button.textContent = answer;
        button.addEventListener("click", () => selectAnswer(index));
        answerButtons.appendChild(button);
    });
}

function resetState() {
    answerButtons.innerHTML = "";
    nextButton.style.display = "none";
}

function selectAnswer(index) {
    let correct = questions[currentQuestionIndex].correct;
    if (index === correct) {
        score++;
        scoreElement.textContent = score;
    }
    nextButton.style.display = "block";
}

nextButton.addEventListener("click", () => {
    currentQuestionIndex++;
    if (currentQuestionIndex < questions.length) {
        showQuestion();
    } else {
        questionElement.textContent = "Quiz Finished!";
        answerButtons.innerHTML = "";
        nextButton.style.display = "none";
    }
});

document.getElementById("toggle-mode").addEventListener("click", () => {
    document.body.classList.toggle("dark-mode");
});

startQuiz();
"""

# Write files
with open(os.path.join(base_dir, "index.html"), "w") as f:
    f.write(html_content)

with open(os.path.join(base_dir, "style.css"), "w") as f:
    f.write(css_content)

with open(os.path.join(base_dir, "script.js"), "w") as f:
    f.write(js_content)

# Create zip file
zip_path = "/mnt/data/mri_quiz_web.zip"
with zipfile.ZipFile(zip_path, "w") as zipf:
    for file_name in os.listdir(base_dir):
        zipf.write(os.path.join(base_dir, file_name), file_name)

zip_path


