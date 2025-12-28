import React, { useState, useEffect, useRef, useMemo } from 'react';
import { 
  ShieldCheck, BookOpen, Car, Scale, RotateCcw, 
  ChevronRight, AlertCircle, Loader2, Target, 
  Lightbulb, Check, X, Percent, 
  ShieldAlert, Infinity, Crosshair,
  Award, TrendingUp, History, Info, Cloud, Database
} from 'lucide-react';

import { initializeApp } from 'firebase/app';
import { 
  getAuth, 
  signInAnonymously, 
  onAuthStateChanged 
} from 'firebase/auth';
import { 
  getFirestore, 
  collection, 
  addDoc, 
  onSnapshot, 
  query, 
  serverTimestamp 
} from 'firebase/firestore';

// ==================================================================
// CONFIGURAÇÃO DO FIREBASE (JÁ PREENCHIDA CORRETAMENTE)
// ==================================================================

const firebaseConfig = {
  apiKey: "AIzaSyBrXdHCEQfF-WTTIc89hG2KfAMM8bhFdfs",
  authDomain: "prf-elite.firebaseapp.com",
  projectId: "prf-elite-002",
  storageBucket: "prf-elite.firebasestorage.app",
  messagingSenderId: "582154402260",
  appId: "1:582154402260:web:d58a84bb2c50751e6bbe41",
  measurementId: "G-ZCNV654N5W"
};

// ==================================================================

// Inicialização segura
const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
const appId = 'prf-elite-prod';

const BLOCKS = {
  BLOCO_I: {
    title: "Bloco I - Básicas",
    icon: <BookOpen className="w-5 h-5" />,
    subjects: ["Português", "Raciocínio Lógico-Matemático", "Informática", "Física", "Ética e Cidadania", "Geopolítica", "História da PRF"]
  },
  BLOCO_II: {
    title: "Bloco II - Trânsito",
    icon: <Car className="w-5 h-5" />,
    subjects: ["CTB + Resoluções CONTRAN"]
  },
  BLOCO_III: {
    title: "Bloco III - Direito",
    icon: <Scale className="w-5 h-5" />,
    subjects: ["Direito Constitucional", "Direito Administrativo", "Direito Penal", "Processo Penal", "Direitos Humanos"]
  }
};

const App = () => {
  const [user, setUser] = useState(null);
  const [userStats, setUserStats] = useState({ totalAnswered: 0, correct: 0, incorrect: 0, globalScore: 0 });
  const [isSyncing, setIsSyncing] = useState(true);
  const [appState, setAppState] = useState('config'); 
  const [selectedBlock, setSelectedBlock] = useState('BLOCO_I');
  const [selectedSubject, setSelectedSubject] = useState('Português');
  const [customTopic, setCustomTopic] = useState('');
  const [numQuestions, setNumQuestions] = useState(5);
  const [marathonMode, setMarathonMode] = useState(false);
  
  const [questions, setQuestions] = useState([]);
  const [currentIndex, setCurrentIndex] = useState(0);
  const [answers, setAnswers] = useState({});
  const [seconds, setSeconds] = useState(0);
  const [loadingStatus, setLoadingStatus] = useState("");
  const [error, setError] = useState(null);
  const [isLoadingMore, setIsLoadingMore] = useState(false);
  
  const timerRef = useRef(null);
  const scrollRef = useRef(null);

  useEffect(() => {
    const initAuth = async () => {
      try {
        await signInAnonymously(auth);
      } catch (error) {
        console.error("Auth Error:", error);
        setError("Erro de Login no Banco de Dados");
        setIsSyncing(false);
      }
    };
    initAuth();
    
    return onAuthStateChanged(auth, (u) => {
      setUser(u);
      if (!u) setIsSyncing(false);
    });
  }, []);

  useEffect(() => {
    if (!user) return;
    const q = query(collection(db, 'artifacts', appId, 'users', user.uid, 'history'));
    const unsubscribe = onSnapshot(q, (snapshot) => {
      let total = 0, correct = 0, incorrect = 0, score = 0;
      snapshot.forEach(doc => {
        const d = doc.data();
        total += d.totalQuestions || 0;
        correct += d.correctCount || 0;
        incorrect += d.incorrectCount || 0;
        score += (d.correctCount || 0) - (d.incorrectCount || 0);
      });
      setUserStats({ totalAnswered: total, correct, incorrect, globalScore: score });
      setIsSyncing(false);
    }, (err) => { console.error(err); setIsSyncing(false); });
    return () => unsubscribe();
  }, [user]);

  useEffect(() => {
    if (appState === 'quiz') timerRef.current = setInterval(() => setSeconds(s => s + 1), 1000);
    else clearInterval(timerRef.current);
    return () => clearInterval(timerRef.current);
  }, [appState]);

  useEffect(() => {
    if (scrollRef.current) scrollRef.current.scrollTo({ top: 0, behavior: 'smooth' });
  }, [currentIndex]);

  const formatTime = (s) => {
    const m = Math.floor(s / 60);
    const sec = s % 60;
    return `${m.toString().padStart(2, '0')}:${sec.toString().padStart(2, '0')}`;
  };

  const startMission = async (isLoadingBatch = false) => {
    if (!isLoadingBatch) {
      setAppState('loading');
      setQuestions([]);
      setAnswers({});
      setSeconds(0);
      setCurrentIndex(0);
    } else {
      setIsLoadingMore(true);
    }
    
    setError(null);
    setLoadingStatus("Contactando QG...");

    const historyContext = questions.length > 0 
      ? `Evite: ${questions.slice(-10).map(q => q.item.substring(0, 30)).join(', ')}.`
      : '';

    const payload = {
      contents: [{ parts: [{ text: `Gere ${numQuestions} itens (C/E) de ${selectedSubject}. ${customTopic} ${historyContext}` }] }],
      systemInstruction: { 
        parts: [{ 
          text: `Aja como banca CEBRASPE. JSON puro: { "caderno": [{ "contexto": "...", "pergunta": "...", "gabarito": "C" ou "E", "fundamentacao": "..." }] }` 
        }] 
      },
      generationConfig: { responseMimeType: "application/json" }
    };

    try {
      const result = await fetch('/api/generate', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload)
      });

      const json = await result.json();
      if (json.error) throw new Error(json.error);

      const data = JSON.parse(json.candidates[0].content.parts[0].text);
      const newBatch = data.caderno.map(it => ({
        contexto: it.contexto,
        item: it.pergunta,
        gabarito: it.gabarito,
        explanation: it.fundamentacao
      }));

      if (isLoadingBatch) {
        setQuestions(prev => [...prev, ...newBatch]);
        setIsLoadingMore(false);
      } else {
        setQuestions(newBatch);
        setAppState('quiz');
      }
    } catch (err) {
      console.error(err);
      setError("Erro de Conexão. Verifique a chave GOOGLE_API_KEY na Vercel.");
      if (!isLoadingBatch) setAppState('config');
      setIsLoadingMore(false);
    }
  };

  const handleChoice = (choice) => {
    if (answers[currentIndex]) return;
    setAnswers(prev => ({
      ...prev,
      [currentIndex]: { choice, isCorrect: choice === questions[currentIndex].gabarito }
    }));
  };

  const finishQuiz = async () => {
    const cor = Object.values(answers).filter(a => a.isCorrect).length;
    const inc = Object.values(answers).length - cor;
    if (user) {
        await addDoc(collection(db, 'artifacts', appId, 'users', user.uid, 'history'), {
            timestamp: serverTimestamp(),
            subject: selectedSubject,
            totalQuestions: questions.length,
            correctCount: cor,
            incorrectCount: inc,
            score: cor - inc,
            mode: marathonMode ? 'Maratona' : 'Simulado'
        });
    }
    setAppState('results');
  };

  const results = useMemo(() => {
    const answered = Object.values(answers);
    const correct = answered.filter(a => a.isCorrect).length;
    const incorrect = answered.length - correct;
    const accuracy = questions.length > 0 ? (correct / questions.length) * 100 : 0;
    return { correct, incorrect, score: correct - incorrect, accuracy };
  }, [answers, questions]);

  const isAnswered = answers[currentIndex] !== undefined;
  const isLast = currentIndex === questions.length - 1;

  if (isSyncing) {
     return (
        <div className="min-h-screen bg-[#05080f] flex items-center justify-center flex-col gap-4">
            <Loader2 className="w-12 h-12 text-blue-500 animate-spin" />
            <p className="text-blue-500 font-bold text-[10px] uppercase">Conectando...</p>
        </div>
     );
  }

  return (
    <div className="min-h-screen bg-[#05080f] text-slate-300 font-sans flex flex-col antialiased">
      {appState === 'config' && (
        <div className="flex-grow flex flex-col items-center justify-center p-6">
          <div className="max-w-xl w-full space-y-8">
            <div className="text-center">
              <h1 className="text-5xl font-black italic text-white uppercase">PRF <span className="text-blue-500">Elite</span></h1>
              <p className="text-slate-500 font-bold uppercase text-[10px] mt-2">Versão V3.0 (Segura)</p>
            </div>

            <div className="bg-[#0c1220] border border-white/5 rounded-[2.5rem] p-8 space-y-6 shadow-2xl">
               <div className="grid grid-cols-3 gap-4 mb-4">
                  <div className="text-center"><div className="text-2xl font-black text-white">{userStats.totalAnswered}</div><div className="text-[8px] uppercase text-slate-500">Total</div></div>
                  <div className="text-center"><div className="text-2xl font-black text-green-400">{userStats.correct}</div><div className="text-[8px] uppercase text-slate-500">Acertos</div></div>
                  <div className="text-center"><div className="text-2xl font-black text-blue-400">{userStats.globalScore}</div><div className="text-[8px] uppercase text-slate-500">Líquido</div></div>
               </div>

               <div className="space-y-4">
                <label className="text-[10px] font-black text-blue-500 uppercase">Matéria</label>
                <select value={selectedSubject} onChange={(e) => setSelectedSubject(e.target.value)} className="w-full bg-slate-900 border border-slate-800 p-4 rounded-xl font-bold text-sm text-white">
                  {BLOCKS[selectedBlock].subjects.map(s => <option key={s} value={s}>{s}</option>)}
                </select>
              </div>

              <div className="grid grid-cols-2 gap-4">
                  <div className="space-y-2"><label className="text-[10px] font-black text-blue-500 uppercase">Qtd</label><div className="flex bg-slate-900 rounded-xl border border-slate-800 p-1">{[5, 10, 20].map(n => (<button key={n} onClick={() => setNumQuestions(n)} className={`flex-1 rounded-lg font-black text-[10px] py-3 ${numQuestions === n ? 'bg-blue-600 text-white' : 'text-slate-600'}`}>{n}</button>))}</div></div>
                  <div className="space-y-2"><label className="text-[10px] font-black text-blue-500 uppercase">Modo</label><button onClick={() => setMarathonMode(!marathonMode)} className={`w-full py-3 h-[50px] rounded-xl border-2 flex items-center justify-center gap-2 ${marathonMode ? 'border-blue-500 bg-blue-500/10 text-white' : 'border-slate-800 text-slate-600'}`}><Infinity className="w-4 h-4" /><span className="text-[10px] font-black uppercase">{marathonMode ? 'ON' : 'OFF'}</span></button></div>
              </div>

              {error && <div className="p-4 bg-red-500/10 border border-red-500/20 rounded-xl text-red-500 text-[10px] font-black flex items-center gap-2"><AlertCircle className="w-4 h-4" /> {error}</div>}

              <button onClick={() => startMission(false)} className="w-full py-6 bg-blue-600 text-white rounded-[2rem] font-black text-lg shadow-lg flex items-center justify-center gap-3 uppercase italic active:scale-95 transition-all">
                <Crosshair className="w-6 h-6" /> INICIAR
              </button>
            </div>
          </div>
        </div>
      )}

      {appState === 'loading' && (
        <div className="flex-grow flex flex-col items-center justify-center p-8 space-y-8">
          <Loader2 className="w-16 h-16 text-blue-500 animate-spin" />
          <h2 className="text-2xl font-black text-white uppercase text-center">{loadingStatus}</h2>
          <p className="text-slate-500 font-bold uppercase tracking-widest text-[9px] animate-pulse">A IA está a processar o lote tático</p>
        </div>
      )}

      {appState === 'quiz' && (
        <div className="flex-grow flex flex-col bg-[#05080f]">
          <header className="sticky top-0 z-50 bg-[#05080f]/95 border-b border-white/5 p-4 flex justify-between items-center">
            <div className="flex items-center gap-4">
                <div className="w-10 h-10 rounded-xl bg-blue-600/10 flex items-center justify-center text-blue-500"><Target className="w-5 h-5" /></div>
                <div><h3 className="text-xs font-black text-white uppercase">{selectedSubject}</h3><span className="text-[8px] text-slate-500 font-bold uppercase">Item {currentIndex + 1}</span></div>
            </div>
            <div className="flex items-center gap-4">
                <div className="text-white font-mono text-xl font-black italic">{formatTime(seconds)}</div>
                <button onClick={finishQuiz} className="bg-red-500/10 text-red-500 px-4 py-2 rounded-xl text-[9px] font-black border border-red-500/20 uppercase">Sair</button>
            </div>
          </header>

          <main ref={scrollRef} className="flex-grow overflow-y-auto p-4 pb-32">
            <div className="max-w-3xl mx-auto bg-[#0c1220] rounded-[2rem] border border-white/5 shadow-2xl p-8 space-y-8">
                <div className="bg-slate-900/30 p-6 rounded-2xl border border-white/5 italic text-slate-400 text-sm leading-relaxed">"{questions[currentIndex].contexto}"</div>
                <h4 className="text-xl font-bold text-white">{questions[currentIndex].item}</h4>
                <div className="grid grid-cols-2 gap-4">
                    <button onClick={() => handleChoice('C')} disabled={isAnswered} className={`py-10 rounded-2xl border-2 font-black text-3xl ${isAnswered && questions[currentIndex].gabarito === 'C' ? 'bg-green-600 border-green-500 text-white' : isAnswered && answers[currentIndex]?.choice === 'C' ? 'bg-red-600 border-red-500 text-white' : 'border-slate-800 text-slate-500'}`}>C</button>
                    <button onClick={() => handleChoice('E')} disabled={isAnswered} className={`py-10 rounded-2xl border-2 font-black text-3xl ${isAnswered && questions[currentIndex].gabarito === 'E' ? 'bg-green-600 border-green-500 text-white' : isAnswered && answers[currentIndex]?.choice === 'E' ? 'bg-red-600 border-red-500 text-white' : 'border-slate-800 text-slate-500'}`}>E</button>
                </div>
                {isAnswered && <div className="p-6 bg-blue-600/5 border border-blue-500/10 rounded-2xl text-slate-300 text-sm italic"><div className="flex items-center gap-2 text-blue-500 font-black uppercase text-[10px] mb-2"><Lightbulb className="w-4 h-4" /> Fundamentação</div>{questions[currentIndex].explanation}</div>}
            </div>
          </main>

          <footer className="fixed bottom-0 left-0 w-full bg-[#05080f]/95 border-t border-white/5 p-4 flex justify-center gap-4">
             <button onClick={() => currentIndex === questions.length -1 ? (marathonMode ? startMission(true) : finishQuiz()) : setCurrentIndex(currentIndex + 1)} disabled={isLoadingMore} className="flex-1 max-w-xs py-4 bg-blue-600 text-white rounded-xl font-black text-xs uppercase shadow-lg flex items-center justify-center gap-2">
               {isLoadingMore ? <Loader2 className="w-4 h-4 animate-spin" /> : (isLastQuestion ? (marathonMode ? 'Pedir Reforço' : 'Finalizar') : 'Próxima')} <ChevronRight className="w-4 h-4" />
             </button>
          </footer>
        </div>
      )}

      {appState === 'results' && (
        <div className="flex-grow flex flex-col p-8 items-center justify-center text-center">
            <Award className="w-20 h-20 text-yellow-500 mb-4" />
            <h2 className="text-4xl font-black text-white italic uppercase mb-8">Relatório</h2>
            <div className="grid grid-cols-2 gap-4 w-full max-w-md mb-8">
              <div className="bg-[#0c1220] p-6 rounded-2xl border border-white/5"><div className="text-3xl font-black text-blue-500">{results.score}</div><div className="text-[10px] uppercase font-bold text-slate-500">Líquido</div></div>
              <div className="bg-[#0c1220] p-6 rounded-2xl border border-white/5"><div className="text-3xl font-black text-purple-500">{results.accuracy.toFixed(0)}%</div><div className="text-[10px] uppercase font-bold text-slate-500">Precisão</div></div>
            </div>
            <button onClick={() => setAppState('config')} className="px-12 py-4 bg-white text-black font-black rounded-full uppercase">Reiniciar</button>
        </div>
      )}
    </div>
  );
};

export default App;


