# quiz-
import React, { useState } from 'react';

const questions = [
  {
    id: 1,
    text: 'What is 2 + 2?',
    options: ['3', '4', '5', '6'],
    correctAnswer: '4',
  },
  {
    id: 2,
    text: 'What is the capital of France?',
    options: ['London', 'Paris', 'Berlin', 'Rome'],
    correctAnswer: 'Paris',
  },
  {
    id: 3,
    text: 'Which language runs in a web browser?',
    options: ['Java', 'C', 'Python', 'JavaScript'],
    correctAnswer: 'JavaScript',
  },
  {
    id: 4,
    text: 'What is the square root of 16?',
    options: ['2', '4', '6', '8'],
    correctAnswer: '4',
  },
  {
    id: 5,
    text: 'Which planet is known as the Red Planet?',
    options: ['Earth', 'Mars', 'Jupiter', 'Saturn'],
    correctAnswer: 'Mars',
  },
];

const getRandomQuestion = (prevId) => {
  let remaining = questions.filter((q) => q.id !== prevId);
  return remaining[Math.floor(Math.random() * remaining.length)];
};

export default function QuizApp() {
  const [currentQuestion, setCurrentQuestion] = useState(getRandomQuestion(null));
  const [selectedAnswer, setSelectedAnswer] = useState(null);
  const [feedback, setFeedback] = useState('');
  const [score, setScore] = useState(0);
  const [questionCount, setQuestionCount] = useState(1);
  const [lastQuestionId, setLastQuestionId] = useState(currentQuestion.id);
  const [showResult, setShowResult] = useState(false);

  const handleAnswerSubmit = () => {
    if (!selectedAnswer) return;
    if (selectedAnswer === currentQuestion.correctAnswer) {
      setScore((prev) => prev + 1);
      setFeedback('✅ Correct!');
    } else {
      setFeedback(`❌ Incorrect! Correct Answer: ${currentQuestion.correctAnswer}`);
    }
  };

  const handleNextQuestion = () => {
    if (questionCount >= 5) {
      setShowResult(true);
    } else {
      const nextQ = getRandomQuestion(lastQuestionId);
      setCurrentQuestion(nextQ);
      setLastQuestionId(nextQ.id);
      setSelectedAnswer(null);
      setFeedback('');
      setQuestionCount((prev) => prev + 1);
    }
  };

  const handleRestart = () => {
    const newQ = getRandomQuestion(null);
    setCurrentQuestion(newQ);
    setLastQuestionId(newQ.id);
    setSelectedAnswer(null);
    setFeedback('');
    setScore(0);
    setQuestionCount(1);
    setShowResult(false);
  };

  return (
    <div className="max-w-xl mx-auto mt-10 p-6 bg-white shadow-xl rounded-xl">
      <h1 className="text-2xl font-bold mb-4">🧠 Quiz App</h1>
      {showResult ? (
        <div className="text-center">
          <p className="text-xl font-semibold">🎉 Quiz Completed!</p>
          <p className="text-lg mt-2">Your Score: {score} / 5</p>
          <button
            className="mt-4 px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
            onClick={handleRestart}
          >
            Restart Quiz
          </button>
        </div>
      ) : (
        <>
          <div className="mb-4">
            <p className="text-lg font-medium">
              Q{questionCount}: {currentQuestion.text}
            </p>
            <div className="mt-2 space-y-2">
              {currentQuestion.options.map((opt, idx) => (
                <label key={idx} className="block">
                  <input
                    type="radio"
                    name="option"
                    value={opt}
                    checked={selectedAnswer === opt}
                    onChange={() => setSelectedAnswer(opt)}
                    className="mr-2"
                  />
                  {opt}
                </label>
              ))}
            </div>
          </div>

          {feedback && <p className="mb-2 font-medium">{feedback}</p>}

          <div className="flex gap-4">
            <button
              className="px-4 py-2 bg-green-600 text-white rounded hover:bg-green-700"
              onClick={handleAnswerSubmit}
              disabled={!selectedAnswer}
            >
              Submit Answer
            </button>
            <button
              className="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
              onClick={handleNextQuestion}
              disabled={!feedback}
            >
              Next Question
            </button>
          </div>

          <p className="mt-4 text-gray-700">Score: {score}</p>
        </>
      )}
    </div>
  );
}
