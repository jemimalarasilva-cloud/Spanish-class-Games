# mis-juegos-de-gemini
Juegos de espanol
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Service & Care - Oppenheimer Park</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        .preserve-3d { transform-style: preserve-3d; }
        .backface-hidden { backface-visibility: hidden; }
    </style>
</head>
<body>
    <div id="root"></div>
    <script type="text/babel">
        const { useState, useEffect, useCallback } = React;
        const Icon = ({ name, color, size = 20 }) => {
            useEffect(() => { if (window.lucide) window.lucide.createIcons(); }, [name]);
            return <i data-lucide={name} style={{ color: color, width: size, height: size }}></i>;
        };

        const PHRASES = [
            { id: 1, en: "Coffee or tea?", es: "¿Café o té?", icon: "coffee", color: "#b45309" },
            { id: 2, en: "Sugar or milk?", es: "¿Azúcar o leche?", icon: "coffee", color: "#f59e0b" },
            { id: 3, en: "It is hot!", es: "¡Está caliente!", icon: "info", color: "#ef4444" },
            { id: 4, en: "Good morning", es: "Buenos días", icon: "heart", color: "#ec4899" }
        ];

        const App = () => {
            const [cards, setCards] = useState([]);
            const [flipped, setFlipped] = useState([]);
            const [solved, setSolved] = useState([]);

            useEffect(() => {
                const gameCards = [];
                PHRASES.forEach(p => {
                    gameCards.push({ id: `en-${p.id}`, content: p.en, matchId: p.id, lang: 'EN' });
                    gameCards.push({ id: `es-${p.id}`, content: p.es, matchId: p.id, lang: 'ES' });
                });
                setCards(gameCards.sort(() => Math.random() - 0.5));
            }, []);

            const handleCardClick = (id) => {
                if (flipped.includes(id) || solved.includes(id) || flipped.length === 2) return;
                const newFlipped = [...flipped, id];
                setFlipped(newFlipped);
                if (newFlipped.length === 2) {
                    const first = cards.find(c => c.id === newFlipped[0]);
                    const second = cards.find(c => c.id === newFlipped[1]);
                    if (first.matchId === second.matchId && first.lang !== second.lang) {
                        setSolved([...solved, first.id, second.id]);
                        setFlipped([]);
                    } else {
                        setTimeout(() => setFlipped([]), 1000);
                    }
                }
            };

            return (
                <div className="min-h-screen bg-slate-100 p-8">
                    <h1 className="text-2xl font-black text-center mb-8 uppercase tracking-tighter">Oppenheimer Park Game</h1>
                    <div className="grid grid-cols-2 sm:grid-cols-4 gap-4 max-w-2xl mx-auto">
                        {cards.map(card => (
                            <div key={card.id} onClick={() => handleCardClick(card.id)} className={`relative h-32 cursor-pointer transition-all duration-500 preserve-3d ${flipped.includes(card.id) || solved.includes(card.id) ? '[transform:rotateY(180deg)]' : ''}`}>
                                <div className="absolute inset-0 bg-white border-2 border-slate-200 rounded-2xl flex items-center justify-center backface-hidden shadow-sm font-bold text-slate-300 text-2xl">?</div>
                                <div className={`absolute inset-0 [transform:rotateY(180deg)] backface-hidden rounded-2xl border-2 p-4 flex flex-col items-center justify-center text-center shadow-md ${card.lang === 'EN' ? 'bg-blue-50 border-blue-400' : 'bg-rose-50 border-rose-400'}`}>
                                    <p className="text-sm font-black">{card.content}</p>
                                </div>
                            </div>
                        ))}
                    </div>
                </div>
            );
        };
        ReactDOM.createRoot(document.getElementById('root')).render(<App />);
    </script>
</body>
</html>
