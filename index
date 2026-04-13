import React, { useState, useRef, useEffect } from 'react';
import { Activity, BrainCircuit, Calculator, CheckCircle2, XCircle, ArrowRight, Trophy, Sparkles, MessageSquare, Loader2, Send } from 'lucide-react';

const apiKey = ""; // API key is provided by the execution environment

// --- Utility: Gemini API Caller with Exponential Backoff ---
const callGeminiAPI = async (prompt, systemInstruction = "", isJson = false) => {
  const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`;
  
  const payload = {
    contents: [{ parts: [{ text: prompt }] }],
  };

  if (systemInstruction) {
    payload.systemInstruction = {
      parts: [{ text: systemInstruction }]
    };
  }

  if (isJson) {
    payload.generationConfig = {
      responseMimeType: "application/json",
      responseSchema: {
        type: "OBJECT",
        properties: {
          text: { type: "STRING", description: "A short biomechanics scenario statement." },
          type: { type: "STRING", description: "Must be exactly 'Qualitative' or 'Quantitative'" }
        },
        required: ["text", "type"]
      }
    };
  }

  let retries = 5;
  let delay = 1000;

  while (retries > 0) {
    try {
      const response = await fetch(url, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload)
      });

      if (!response.ok) throw new Error(`HTTP error! status: ${response.status}`);
      
      const data = await response.json();
      const textResponse = data.candidates?.[0]?.content?.parts?.[0]?.text;
      
      return isJson ? JSON.parse(textResponse) : textResponse;
    } catch (error) {
      retries--;
      if (retries === 0) throw error;
      await new Promise(res => setTimeout(res, delay));
      delay *= 2;
    }
  }
};

export default function App() {
  const [activeTab, setActiveTab] = useState('terminology');

  return (
    <div className="min-h-screen bg-slate-50 p-4 md:p-8 font-sans text-slate-800">
      <div className="max-w-5xl mx-auto">
        {/* Header */}
        <div className="bg-blue-600 text-white p-6 rounded-t-2xl shadow-lg mb-6">
          <h1 className="text-3xl font-bold mb-2">Fundamental of Sport Biomechanics</h1>
          <p className="text-blue-100">Interactive Learning Activity based on Chapter 1</p>
        </div>

        {/* Navigation Tabs */}
        <div className="flex flex-wrap gap-2 mb-6">
          <TabButton 
            active={activeTab === 'terminology'} 
            onClick={() => setActiveTab('terminology')} 
            icon={<BrainCircuit size={18} />} 
            label="1. Terminology" 
          />
          <TabButton 
            active={activeTab === 'analysis'} 
            onClick={() => setActiveTab('analysis')} 
            icon={<Activity size={18} />} 
            label="2. Analysis Types" 
          />
          <TabButton 
            active={activeTab === 'math'} 
            onClick={() => setActiveTab('math')} 
            icon={<Calculator size={18} />} 
            label="3. Baseball Problem" 
          />
          <TabButton 
            active={activeTab === 'tutor'} 
            onClick={() => setActiveTab('tutor')} 
            icon={<MessageSquare size={18} />} 
            label="4. AI Tutor" 
          />
        </div>

        {/* Content Area */}
        <div className="bg-white rounded-2xl shadow-md p-6 min-h-[400px]">
          {activeTab === 'terminology' && <TerminologyQuiz />}
          {activeTab === 'analysis' && <QualitativeVsQuantitative />}
          {activeTab === 'math' && <BaseballProblem />}
          {activeTab === 'tutor' && <AiTutor />}
        </div>
      </div>
    </div>
  );
}

// --- TAB COMPONENT ---
const TabButton = ({ active, onClick, icon, label }) => (
  <button
    onClick={onClick}
    className={`flex items-center gap-2 px-4 py-3 rounded-lg font-semibold transition-all duration-200 ${
      active 
        ? 'bg-blue-600 text-white shadow-md transform scale-105' 
        : 'bg-white text-slate-600 hover:bg-blue-50 border border-slate-200'
    }`}
  >
    {icon}
    {label}
  </button>
);

// --- MODULE 1: TERMINOLOGY QUIZ ---
const TerminologyQuiz = () => {
  const questions = [
    {
      question: "The study of systems that are in a state of constant motion (at rest or moving with constant velocity).",
      options: ["Dynamics", "Statics", "Kinematics", "Kinetics"],
      answer: "Statics"
    },
    {
      question: "Describes the appearance of motion (pattern, speed, sequencing, and coordination).",
      options: ["Kinematics", "Kinetics", "Kinesiology", "Statics"],
      answer: "Kinematics"
    },
    {
      question: "The study of the forces associated with motion.",
      options: ["Dynamics", "Statics", "Kinematics", "Kinetics"],
      answer: "Kinetics"
    },
    {
      question: "The study of systems in which acceleration is present.",
      options: ["Dynamics", "Statics", "Kinematics", "Kinetics"],
      answer: "Dynamics"
    }
  ];

  const [currentQ, setCurrentQ] = useState(0);
  const [selected, setSelected] = useState(null);
  const [score, setScore] = useState(0);
  const [showResult, setShowResult] = useState(false);

  const handleSelect = (option) => {
    if (selected) return; // Prevent multiple clicks
    setSelected(option);
    if (option === questions[currentQ].answer) {
      setScore(score + 1);
    }
  };

  const nextQuestion = () => {
    if (currentQ < questions.length - 1) {
      setCurrentQ(currentQ + 1);
      setSelected(null);
    } else {
      setShowResult(true);
    }
  };

  const reset = () => {
    setCurrentQ(0);
    setSelected(null);
    setScore(0);
    setShowResult(false);
  };

  if (showResult) {
    return (
      <div className="text-center py-10 animate-fade-in">
        <Trophy size={64} className="mx-auto text-yellow-400 mb-4" />
        <h2 className="text-2xl font-bold mb-2">Quiz Complete!</h2>
        <p className="text-lg mb-6">You scored {score} out of {questions.length}</p>
        <button onClick={reset} className="bg-blue-600 text-white px-6 py-2 rounded-lg hover:bg-blue-700">
          Try Again
        </button>
      </div>
    );
  }

  const q = questions[currentQ];

  return (
    <div className="animate-fade-in">
      <h2 className="text-xl font-bold mb-6 text-blue-800 border-b pb-2">Sub-branches of Mechanics</h2>
      <div className="mb-4 text-sm text-slate-500 font-bold">Question {currentQ + 1} of {questions.length}</div>
      <p className="text-lg mb-6">{q.question}</p>
      
      <div className="grid grid-cols-1 md:grid-cols-2 gap-4 mb-8">
        {q.options.map((opt) => {
          let btnClass = "border-2 border-slate-200 bg-white hover:bg-slate-50 text-slate-700";
          if (selected) {
            if (opt === q.answer) btnClass = "border-2 border-green-500 bg-green-50 text-green-700";
            else if (opt === selected) btnClass = "border-2 border-red-500 bg-red-50 text-red-700";
            else btnClass = "border-2 border-slate-100 bg-slate-50 text-slate-400 opacity-50";
          }

          return (
            <button
              key={opt}
              onClick={() => handleSelect(opt)}
              disabled={selected !== null}
              className={`p-4 rounded-xl font-semibold text-left transition-all ${btnClass}`}
            >
              {opt}
            </button>
          );
        })}
      </div>

      {selected && (
        <div className="flex justify-end">
          <button onClick={nextQuestion} className="flex items-center gap-2 bg-blue-600 text-white px-6 py-3 rounded-lg hover:bg-blue-700">
            {currentQ < questions.length - 1 ? 'Next Question' : 'View Results'} <ArrowRight size={18} />
          </button>
        </div>
      )}
    </div>
  );
};

// --- MODULE 2: QUALITATIVE VS QUANTITATIVE (AI POWERED) ---
const QualitativeVsQuantitative = () => {
  const defaultScenarios = [
    { text: "The robot missed the coffee cup by 15 cm.", type: "Quantitative" },
    { text: "The robot malfunctioned.", type: "Qualitative" },
    { text: "Is the movement being performed with adequate force?", type: "Qualitative" },
    { text: "The runner's speed was 9 m/s.", type: "Quantitative" },
    { text: "Is there excessive pronation taking place during the stance phase?", type: "Qualitative" },
  ];

  const [scenarios, setScenarios] = useState(defaultScenarios);
  const [currentIndex, setCurrentIndex] = useState(0);
  const [feedback, setFeedback] = useState(null);
  const [score, setScore] = useState(0);
  const [isComplete, setIsComplete] = useState(false);
  const [isGenerating, setIsGenerating] = useState(false);
  const [aiError, setAiError] = useState("");

  const handleGuess = (guess) => {
    const isCorrect = guess === scenarios[currentIndex].type;
    setFeedback({ isCorrect, correctType: scenarios[currentIndex].type });
    if (isCorrect) setScore(score + 1);

    setTimeout(() => {
      setFeedback(null);
      if (currentIndex < scenarios.length - 1) {
        setCurrentIndex((prev) => prev + 1);
      } else {
        setIsComplete(true);
      }
    }, 1500);
  };

  const generateNewScenario = async () => {
    setIsGenerating(true);
    setAiError("");
    try {
      const prompt = "Generate a single random scenario statement related to sports biomechanics. It should clearly be either a 'Qualitative' observation/question or a 'Quantitative' measurement. Return as JSON.";
      const newScenario = await callGeminiAPI(prompt, "", true);
      
      if (newScenario && newScenario.text && newScenario.type) {
        // Insert the new scenario right after the current one, or replace if complete
        if (isComplete) {
          setScenarios([newScenario]);
          setCurrentIndex(0);
          setIsComplete(false);
        } else {
          const updatedScenarios = [...scenarios];
          updatedScenarios.splice(currentIndex + 1, 0, newScenario);
          setScenarios(updatedScenarios);
          setCurrentIndex(currentIndex + 1);
        }
      }
    } catch (err) {
      setAiError("Failed to generate a new scenario. Please try again.");
    } finally {
      setIsGenerating(false);
    }
  };

  const reset = () => {
    setScenarios(defaultScenarios);
    setCurrentIndex(0);
    setScore(0);
    setFeedback(null);
    setIsComplete(false);
  };

  if (isComplete) {
    return (
      <div className="text-center py-10 animate-fade-in">
        <Trophy size={64} className="mx-auto text-yellow-400 mb-4" />
        <h2 className="text-2xl font-bold mb-2">Classification Complete!</h2>
        <p className="text-lg mb-6">You scored {score} out of {scenarios.length}</p>
        <div className="flex justify-center gap-4">
          <button onClick={reset} className="bg-slate-200 text-slate-800 px-6 py-2 rounded-lg hover:bg-slate-300 font-semibold">
            Restart Default Quiz
          </button>
          <button 
            onClick={generateNewScenario} 
            disabled={isGenerating}
            className="flex items-center gap-2 bg-blue-600 text-white px-6 py-2 rounded-lg hover:bg-blue-700 font-semibold disabled:opacity-75"
          >
            {isGenerating ? <Loader2 className="animate-spin" size={18} /> : <Sparkles size={18} />}
            Generate Endless AI Scenarios ✨
          </button>
        </div>
      </div>
    );
  }

  return (
    <div className="text-center py-4 animate-fade-in">
      <div className="flex justify-between items-center mb-6">
        <h2 className="text-xl font-bold text-blue-800">Problem Solving Classification</h2>
        <button 
          onClick={generateNewScenario}
          disabled={isGenerating || feedback !== null}
          className="flex items-center gap-2 text-sm bg-purple-100 text-purple-700 px-3 py-1.5 rounded-full font-bold hover:bg-purple-200 transition-colors disabled:opacity-50"
        >
          {isGenerating ? <Loader2 className="animate-spin" size={14} /> : <Sparkles size={14} />}
          AI Generate Next ✨
        </button>
      </div>
      
      <div className="mb-4 text-sm text-slate-500 font-bold">Statement {currentIndex + 1} of {scenarios.length}</div>
      <p className="text-slate-600 mb-8">Classify the following statement as Qualitative or Quantitative.</p>

      {aiError && <div className="text-red-500 text-sm mb-4 font-semibold">{aiError}</div>}

      <div className="bg-slate-100 p-8 rounded-2xl mb-8 min-h-[160px] flex items-center justify-center relative shadow-inner">
        {feedback ? (
          <div className={`text-2xl font-bold flex items-center gap-2 ${feedback.isCorrect ? 'text-green-600' : 'text-red-600'}`}>
            {feedback.isCorrect ? <CheckCircle2 size={32} /> : <XCircle size={32} />}
            {feedback.isCorrect ? 'Correct!' : `Incorrect. It is ${feedback.correctType}.`}
          </div>
        ) : (
          <p className="text-2xl font-medium text-slate-800">"{scenarios[currentIndex].text}"</p>
        )}
      </div>

      <div className="flex justify-center gap-4">
        <button 
          onClick={() => handleGuess('Qualitative')}
          disabled={feedback !== null || isGenerating}
          className="flex-1 max-w-[200px] bg-purple-600 hover:bg-purple-700 text-white font-bold py-4 rounded-xl transition-colors disabled:opacity-50 shadow-md"
        >
          Qualitative
        </button>
        <button 
          onClick={() => handleGuess('Quantitative')}
          disabled={feedback !== null || isGenerating}
          className="flex-1 max-w-[200px] bg-teal-600 hover:bg-teal-700 text-white font-bold py-4 rounded-xl transition-colors disabled:opacity-50 shadow-md"
        >
          Quantitative
        </button>
      </div>

      <div className="mt-8 text-slate-500 font-medium">
        Current Score: {score}
      </div>
    </div>
  );
};

// --- MODULE 3: THE BASEBALL PROBLEM ---
const BaseballProblem = () => {
  const [catcherTime, setCatcherTime] = useState('');
  const [runnerTime, setRunnerTime] = useState('');
  const [winner, setWinner] = useState('');
  const [isSubmitted, setIsSubmitted] = useState(false);

  const checkAnswers = () => {
    setIsSubmitted(true);
  };

  const isCatcherCorrect = parseFloat(catcherTime) === 2;
  const isRunnerCorrect = parseFloat(runnerTime) === 1.67;
  const isWinnerCorrect = winner === 'runner';

  return (
    <div className="animate-fade-in">
      <h2 className="text-xl font-bold mb-4 text-blue-800 border-b pb-2">Sample Problem 1: Baseball</h2>
      
      <div className="bg-blue-50 p-4 rounded-xl mb-6 text-sm md:text-base text-blue-900 leading-relaxed border border-blue-200">
        <p>A baseball player hits a triple to deep left field. As he approaches third base, he decides to run for home plate.</p>
        <ul className="list-disc pl-5 mt-2 space-y-1 font-semibold">
          <li>The <strong>catcher</strong> retrieves the ball <strong>10 m</strong> from the plate and runs back at a speed of <strong>5 m/s</strong>.</li>
          <li>The <strong>base runner</strong> is <strong>15 m</strong> from the plate, traveling at a speed of <strong>9 m/s</strong>.</li>
        </ul>
        <p className="mt-2 font-bold italic">Formula: Time = Distance / Speed</p>
      </div>

      <div className="grid md:grid-cols-2 gap-6 mb-8">
        {/* Catcher Calc */}
        <div className="bg-white border-2 border-slate-100 p-4 rounded-xl shadow-sm">
          <h3 className="font-bold text-slate-700 mb-3 flex items-center gap-2">
            <div className="w-8 h-8 rounded-full bg-red-100 text-red-600 flex items-center justify-center">1</div>
            Catcher's Time
          </h3>
          <div className="flex items-center gap-2 mb-2">
            <span className="text-slate-500">10m / 5 m/s = </span>
            <input 
              type="number" 
              value={catcherTime}
              onChange={(e) => setCatcherTime(e.target.value)}
              className="w-20 p-2 border rounded focus:ring-2 outline-none"
              placeholder="0.00"
              disabled={isSubmitted}
            />
            <span className="text-slate-500">s</span>
          </div>
          {isSubmitted && (
            <div className={`text-sm font-bold ${isCatcherCorrect ? 'text-green-600' : 'text-red-500'}`}>
              {isCatcherCorrect ? 'Correct! (2s)' : 'Incorrect. 10 / 5 = 2'}
            </div>
          )}
        </div>

        {/* Runner Calc */}
        <div className="bg-white border-2 border-slate-100 p-4 rounded-xl shadow-sm">
          <h3 className="font-bold text-slate-700 mb-3 flex items-center gap-2">
            <div className="w-8 h-8 rounded-full bg-green-100 text-green-600 flex items-center justify-center">2</div>
            Runner's Time
          </h3>
          <div className="flex items-center gap-2 mb-2">
            <span className="text-slate-500">15m / 9 m/s = </span>
            <input 
              type="number" 
              value={runnerTime}
              onChange={(e) => setRunnerTime(e.target.value)}
              className="w-20 p-2 border rounded focus:ring-2 outline-none"
              placeholder="0.00"
              disabled={isSubmitted}
            />
            <span className="text-slate-500">s</span>
          </div>
          {isSubmitted && (
            <div className={`text-sm font-bold ${isRunnerCorrect ? 'text-green-600' : 'text-red-500'}`}>
              {isRunnerCorrect ? 'Correct! (1.67s)' : 'Incorrect. 15 / 9 = 1.67'}
            </div>
          )}
        </div>
      </div>

      <div className="bg-slate-50 p-4 rounded-xl border border-slate-200 mb-6 text-center">
        <h3 className="font-bold text-lg mb-3">Who will reach the plate first?</h3>
        <div className="flex justify-center gap-4">
          <button 
            onClick={() => setWinner('catcher')}
            disabled={isSubmitted}
            className={`px-6 py-2 rounded-lg font-bold border-2 transition-colors ${
              winner === 'catcher' ? 'bg-red-500 text-white border-red-600 shadow-md' : 'bg-white text-slate-600 border-slate-300 hover:bg-slate-100'
            }`}
          >
            The Catcher
          </button>
          <button 
            onClick={() => setWinner('runner')}
            disabled={isSubmitted}
            className={`px-6 py-2 rounded-lg font-bold border-2 transition-colors ${
              winner === 'runner' ? 'bg-green-500 text-white border-green-600 shadow-md' : 'bg-white text-slate-600 border-slate-300 hover:bg-slate-100'
            }`}
          >
            The Base Runner
          </button>
        </div>
        {isSubmitted && (
          <div className={`mt-4 text-lg font-bold flex justify-center items-center gap-2 ${isWinnerCorrect ? 'text-green-600' : 'text-red-500'}`}>
            {isWinnerCorrect ? <CheckCircle2 /> : <XCircle />}
            {isWinnerCorrect ? 'Correct! The runner arrives first by 0.33 seconds.' : 'Incorrect. The runner takes less time (1.67s vs 2s) so he arrives first.'}
          </div>
        )}
      </div>

      {!isSubmitted ? (
        <button 
          onClick={checkAnswers}
          disabled={!catcherTime || !runnerTime || !winner}
          className="w-full bg-blue-600 text-white font-bold py-3 rounded-lg hover:bg-blue-700 disabled:opacity-50 disabled:cursor-not-allowed shadow-md"
        >
          Check Answers
        </button>
      ) : (
        <button 
          onClick={() => {
            setIsSubmitted(false);
            setCatcherTime('');
            setRunnerTime('');
            setWinner('');
          }}
          className="w-full bg-slate-200 text-slate-800 font-bold py-3 rounded-lg hover:bg-slate-300 transition-colors"
        >
          Reset Problem
        </button>
      )}

    </div>
  );
};

// --- MODULE 4: AI TUTOR (GEMINI API) ---
const AiTutor = () => {
  const [messages, setMessages] = useState([
    { role: 'ai', text: "Hello! I'm your AI Biomechanics Tutor. Ask me anything about kinematics, kinetics, statics, dynamics, or the problem-solving steps we covered in Chapter 1!" }
  ]);
  const [input, setInput] = useState('');
  const [isLoading, setIsLoading] = useState(false);
  const messagesEndRef = useRef(null);

  const scrollToBottom = () => {
    messagesEndRef.current?.scrollIntoView({ behavior: "smooth" });
  };

  useEffect(() => {
    scrollToBottom();
  }, [messages, isLoading]);

  const handleSend = async (e) => {
    e.preventDefault();
    if (!input.trim() || isLoading) return;

    const userText = input.trim();
    setInput('');
    setMessages(prev => [...prev, { role: 'user', text: userText }]);
    setIsLoading(true);

    try {
      const systemInstruction = "You are a helpful and encouraging tutor specializing in Fundamental of Sport Biomechanics for undergraduate students. Focus your answers around Chapter 1 concepts: Statics, Dynamics, Kinematics, Kinetics, Qualitative vs Quantitative analysis, and the 11 sequential steps for solving biomechanics problems. Keep your answers clear, concise, and easy to digest.";
      
      const fullContextPrompt = messages.map(m => `${m.role === 'ai' ? 'Tutor:' : 'Student:'} ${m.text}`).join("\n") + `\nStudent: ${userText}`;
      
      const response = await callGeminiAPI(fullContextPrompt, systemInstruction, false);
      
      setMessages(prev => [...prev, { role: 'ai', text: response }]);
    } catch (error) {
      setMessages(prev => [...prev, { role: 'ai', text: "Sorry, I'm having trouble connecting right now. Please try again later." }]);
    } finally {
      setIsLoading(false);
    }
  };

  return (
    <div className="flex flex-col h-[500px] animate-fade-in">
      <div className="flex items-center gap-2 mb-4 text-blue-800 border-b pb-2">
        <Sparkles size={20} className="text-yellow-500" />
        <h2 className="text-xl font-bold">Ask the Biomechanics Tutor</h2>
      </div>

      {/* Chat History */}
      <div className="flex-1 overflow-y-auto p-4 bg-slate-50 rounded-xl mb-4 border border-slate-200">
        {messages.map((msg, idx) => (
          <div key={idx} className={`mb-4 flex ${msg.role === 'user' ? 'justify-end' : 'justify-start'}`}>
            <div className={`max-w-[80%] p-3 rounded-2xl ${
              msg.role === 'user' 
                ? 'bg-blue-600 text-white rounded-tr-none' 
                : 'bg-white text-slate-700 border border-slate-200 rounded-tl-none shadow-sm'
            }`}>
              <p className="whitespace-pre-wrap text-sm md:text-base">{msg.text}</p>
            </div>
          </div>
        ))}
        {isLoading && (
          <div className="flex justify-start mb-4">
            <div className="bg-white border border-slate-200 p-3 rounded-2xl rounded-tl-none shadow-sm flex items-center gap-2">
              <Loader2 className="animate-spin text-blue-500" size={16} />
              <span className="text-slate-500 text-sm">Thinking...</span>
            </div>
          </div>
        )}
        <div ref={messagesEndRef} />
      </div>

      {/* Input Area */}
      <form onSubmit={handleSend} className="flex gap-2">
        <input 
          type="text" 
          value={input}
          onChange={(e) => setInput(e.target.value)}
          placeholder="Ask a question about Chapter 1..."
          className="flex-1 p-3 rounded-xl border border-slate-300 focus:ring-2 focus:ring-blue-500 focus:outline-none"
          disabled={isLoading}
        />
        <button 
          type="submit" 
          disabled={!input.trim() || isLoading}
          className="bg-blue-600 text-white p-3 rounded-xl hover:bg-blue-700 disabled:opacity-50 transition-colors flex items-center justify-center gap-2 font-semibold shadow-md"
        >
          <span className="hidden md:inline">Ask Tutor ✨</span>
          <Send size={18} />
        </button>
      </form>
    </div>
  );
};
