import React, { useState, useEffect } from 'react';
import { 
  Search, 
  ExternalLink, 
  Coffee, 
  AlertTriangle, 
  Info, 
  BookOpen, 
  Copy, 
  CheckCircle2, 
  Sparkles,
  Zap,
  ChevronRight,
  RefreshCcw
} from 'lucide-react';

const App = () => {
  const [niche, setNiche] = useState('');
  const [loading, setLoading] = useState(false);
  const [keywords, setKeywords] = useState([]);
  const [copiedId, setCopiedId] = useState(null);
  const [error, setError] = useState(null);

  const apiKey = "AIzaSyCuPxYZAO0VdKtn3TZNHx1wt4V-_6QZNAA";

  const fetchWithRetry = async (nicheText, retryCount = 0) => {
    const delays = [1000, 2000, 4000, 8000, 16000];
    const systemPrompt = "You are a professional SEO keyword researcher. Return exactly 6 high-potential keywords for a specific niche in a JSON array format. Each object must have: id (number), text (the keyword), volume (High/Medium/Low), and difficulty (Easy/Medium/Hard). Only return the JSON code block.";
    const userQuery = `Find keywords for the niche: ${nicheText}`;

    try {
      const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          contents: [{ parts: [{ text: userQuery }] }],
          systemInstruction: { parts: [{ text: systemPrompt }] },
          generationConfig: { responseMimeType: "application/json" }
        })
      });

      if (!response.ok) throw new Error('API request failed');

      const result = await response.json();
      const text = result.candidates?.[0]?.content?.parts?.[0]?.text;
      return JSON.parse(text);

    } catch (err) {
      if (retryCount < 5) {
        await new Promise(resolve => setTimeout(resolve, delays[retryCount]));
        return fetchWithRetry(nicheText, retryCount + 1);
      }
      throw new Error('We were unable to connect to the AI after several attempts. Please try again later.');
    }
  };

  const findKeywords = async () => {
    if (!niche.trim()) return;
    
    setLoading(true);
    setError(null);
    setKeywords([]);

    try {
      const data = await fetchWithRetry(niche);
      setKeywords(Array.isArray(data) ? data : data.keywords || []);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  };

  const copyToClipboard = (text, id) => {
    const textArea = document.createElement("textarea");
    textArea.value = text;
    document.body.appendChild(textArea);
    textArea.select();
    try {
      document.execCommand('copy');
      setCopiedId(id);
      setTimeout(() => setCopiedId(null), 2000);
    } catch (err) {
      console.error('Copy failed', err);
    }
    document.body.removeChild(textArea);
  };

  return (
    <div className="min-h-screen bg-gray-50 font-sans text-slate-900 selection:bg-blue-100">
      <div className="bg-blue-900 text-blue-100 py-2 px-4 text-center text-xs font-medium">
        Live AI Analysis enabled for <a href="https://www.fik-rago.top/" className="underline hover:text-white transition">fik-rago.top</a> users.
      </div>

      <nav className="bg-white border-b border-gray-200 px-6 py-4 flex justify-between items-center sticky top-0 z-50 shadow-sm">
        <div className="flex items-center gap-3">
          <div className="bg-gradient-to-br from-blue-600 to-indigo-700 p-2 rounded-xl text-white shadow-lg shadow-blue-200">
            <Sparkles size={22} />
          </div>
          <span className="font-black text-2xl bg-clip-text text-transparent bg-gradient-to-r from-blue-700 to-indigo-900">
            AI Keywords
          </span>
        </div>
        
        <div className="hidden md:flex items-center gap-8 font-semibold text-slate-600">
          <a href="#about" className="hover:text-blue-600 transition flex items-center gap-1">
            <Info size={16} /> About
          </a>
          <a href="#strategy" className="hover:text-blue-600 transition flex items-center gap-1">
            <Zap size={16} /> Strategy
          </a>
          <a 
            href="https://www.paypal.com/paypalme/websiteMA" 
            target="_blank" 
            rel="noopener noreferrer"
            className="bg-amber-400 hover:bg-amber-500 text-amber-950 px-5 py-2.5 rounded-full flex items-center gap-2 shadow-md transition-all active:scale-95"
          >
            <Coffee size={18} /> Support Me
          </a>
        </div>
      </nav>

      <header className="bg-gradient-to-b from-blue-700 to-indigo-800 text-white pt-20 pb-24 px-6 text-center">
        <h1 className="text-4xl md:text-6xl font-extrabold mb-6 tracking-tight">
          AI-Powered <span className="text-amber-400">Niche</span> Discovery
        </h1>
        <p className="text-blue-100 text-lg md:text-xl max-w-2xl mx-auto opacity-90 leading-relaxed">
          Now connected to Gemini 2.5 Flash for real-time insights. 
          Supporting <a href="https://www.fik-rago.top/" className="mx-2 underline decoration-amber-400 underline-offset-4 hover:text-white transition">fik-rago.top</a>
        </p>
      </header>

      <main className="max-w-5xl mx-auto px-6 -mt-16 pb-24">
        <div className="bg-white rounded-3xl shadow-2xl shadow-blue-900/10 p-6 md:p-12 mb-16 border border-white/20 backdrop-blur-sm">
          <div className="flex flex-col md:flex-row gap-4 mb-10">
            <div className="relative flex-1 group">
              <Search className="absolute left-4 top-1/2 -translate-y-1/2 text-slate-400 group-focus-within:text-blue-500 transition-colors" size={24} />
              <input 
                type="text" 
                placeholder="Enter a niche (e.g. Sustainable Fashion, AI for Law)"
                className="w-full pl-12 pr-6 py-4 bg-slate-50 border-2 border-transparent rounded-2xl focus:border-blue-500 focus:bg-white outline-none transition-all text-lg shadow-inner"
                value={niche}
                onChange={(e) => setNiche(e.target.value)}
                onKeyDown={(e) => e.key === 'Enter' && !loading && findKeywords()}
              />
            </div>
            <button 
              onClick={findKeywords}
              disabled={loading || !niche}
              className="bg-blue-600 hover:bg-blue-700 disabled:bg-slate-300 text-white font-bold px-10 py-4 rounded-2xl shadow-xl shadow-blue-600/20 transition-all active:scale-95 flex items-center justify-center gap-2 min-w-[200px]"
            >
              {loading ? (
                <>
                  <RefreshCcw className="w-5 h-5 animate-spin" />
                  Analyzing...
                </>
              ) : (
                <>Ask AI <ChevronRight size={20} /></>
              )}
            </button>
          </div>

          {error && (
            <div className="bg-red-50 text-red-600 p-4 rounded-xl flex items-center gap-3 mb-6 animate-in fade-in zoom-in duration-300">
              <AlertTriangle size={20} />
              <span className="text-sm font-medium">{error}</span>
            </div>
          )}

          {keywords.length > 0 ? (
            <div className="animate-in fade-in slide-in-from-bottom-4 duration-700">
              <div className="flex justify-between items-end mb-6">
                <h3 className="text-xs font-bold text-blue-600 uppercase tracking-widest flex items-center gap-2">
                  <Sparkles size={14} /> AI Suggestions
                </h3>
              </div>
              
              <div className="grid gap-3">
                {keywords.map((kw) => (
                  <div 
                    key={kw.id} 
                    className="flex flex-wrap items-center justify-between p-5 bg-white border border-slate-100 rounded-2xl hover:border-blue-300 hover:shadow-md transition-all group cursor-default"
                  >
                    <div className="flex-1 min-w-[200px]">
                      <span className="font-bold text-slate-700 text-lg group-hover:text-blue-700 transition-colors">
                        {kw.text}
                      </span>
                      <div className="flex gap-4 mt-1">
                        <span className="text-xs font-medium text-slate-400 flex items-center gap-1">
                          Vol: <span className={`font-bold ${kw.volume === 'High' ? 'text-green-600' : 'text-slate-600'}`}>{kw.volume}</span>
                        </span>
                        <span className="text-xs font-medium text-slate-400 flex items-center gap-1">
                          Diff: <span className={`font-bold ${kw.difficulty === 'Easy' ? 'text-green-600' : 'text-slate-600'}`}>{kw.difficulty}</span>
                        </span>
                      </div>
                    </div>
                    <button 
                      onClick={() => copyToClipboard(kw.text, kw.id)}
                      className={`flex items-center gap-2 px-4 py-2 rounded-lg text-sm font-bold transition-all ${
                        copiedId === kw.id 
                          ? 'bg-green-50 text-green-600' 
                          : 'bg-blue-50 text-blue-600 hover:bg-blue-600 hover:text-white'
                      }`}
                    >
                      {copiedId === kw.id ? <><CheckCircle2 size={16} /> Copied</> : <><Copy size={16} /> Copy</>}
                    </button>
                  </div>
                ))}
              </div>
            </div>
          ) : !loading && (
            <div className="text-center py-10 opacity-40">
               <Zap size={48} className="mx-auto mb-4" />
               <p className="text-lg italic">Enter a niche and click Generate to see real AI data</p>
            </div>
          )}
        </div>

        <div className="grid md:grid-cols-2 gap-8">
          <section id="strategy" className="bg-white p-8 rounded-3xl border border-slate-100 shadow-sm">
            <div className="inline-flex p-3 bg-indigo-50 text-indigo-600 rounded-2xl mb-6">
              <BookOpen size={24} />
            </div>
            <h2 className="text-2xl font-bold mb-4">Content Strategy</h2>
            <p className="text-slate-600 leading-relaxed mb-4 text-sm">
              AI-generated keywords need human context. When writing for <a href="https://www.fik-rago.top/" className="text-blue-600 hover:underline">fik-rago.top</a>, focus on E-E-A-T (Experience, Expertise, Authoritativeness, and Trustworthiness).
            </p>
          </section>

          <section id="about" className="bg-white p-8 rounded-3xl border border-slate-100 shadow-sm">
            <div className="inline-flex p-3 bg-blue-50 text-blue-600 rounded-2xl mb-6">
              <Info size={24} />
            </div>
            <h2 className="text-2xl font-bold mb-4">Live AI Integration</h2>
            <p className="text-slate-600 leading-relaxed text-sm">
              This app is now powered by the Gemini 2.5 Flash model. It doesn't just shuffle pre-written ideas; it generates unique suggestions based on current trends and your specific inputs.
            </p>
          </section>
        </div>
      </main>

      <footer className="bg-slate-900 text-slate-400 py-16 px-6">
        <div className="max-w-5xl mx-auto">
          <div className="flex flex-col md:flex-row justify-between gap-12 mb-12 border-b border-slate-800 pb-12">
            <div className="max-w-xs">
              <div className="flex items-center gap-2 text-white font-bold text-xl mb-4">
                <Sparkles size={20} className="text-blue-400" /> AI Keywords
              </div>
              <p className="text-sm">Real-time keyword analysis for independent creators and the fik-rago.top community.</p>
            </div>
            
            <div className="grid grid-cols-2 gap-12">
              <div>
                <h4 className="text-white font-bold mb-6 text-sm uppercase tracking-widest">Connect</h4>
                <ul className="space-y-3 text-sm">
                  <li><a href="https://www.fik-rago.top/" className="hover:text-blue-400 transition flex items-center gap-2"><ExternalLink size={14}/> Website</a></li>
                  <li><a href="https://www.paypal.com/paypalme/websiteMA" className="hover:text-amber-400 transition flex items-center gap-2"><Coffee size={14}/> Donate</a></li>
                </ul>
              </div>
              <div>
                <h4 className="text-white font-bold mb-6 text-sm uppercase tracking-widest">Sitemap</h4>
                <ul className="space-y-3 text-sm">
                  <li><a href="#about" className="hover:text-white transition">About</a></li>
                  <li><a href="#strategy" className="hover:text-white transition">Strategy</a></li>
                </ul>
              </div>
            </div>
          </div>
          
          <div className="flex flex-col gap-8">
            <div className="bg-slate-800/40 p-5 rounded-2xl border border-slate-800 flex flex-col md:flex-row gap-4 items-center">
              <AlertTriangle className="text-amber-500 shrink-0" size={24} />
              <p className="text-xs leading-relaxed text-slate-400">
                <strong>Safety Disclaimer:</strong> AI models like Gemini 2.5 Flash can occasionally generate incorrect or biased information. Always cross-reference keyword difficulty with dedicated SEO tools. Built with ❤️ for the fik-rago.top ecosystem.
              </p>
            </div>
            
            <div className="flex flex-col md:flex-row justify-between items-center gap-4 text-xs font-medium uppercase tracking-tighter">
              <p>© 2025 AI Keyword Research Tool. Partner of fik-rago.top</p>
              <div className="flex gap-6">
                <span>Privacy</span>
                <span>Terms</span>
              </div>
            </div>
          </div>
        </div>
      </footer>
    </div>
  );
};

export default App;
