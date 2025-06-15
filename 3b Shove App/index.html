import React, { useState, useEffect, useMemo } from 'react';

// Helper component for a consistent input slider
const InputSlider = ({ label, value, setValue, min, max, step, unit, disabled = false }) => {
    return (
        <div className={`space-y-2 ${disabled ? 'opacity-50' : ''}`}>
            <div className="flex justify-between items-center">
                <label className="text-sm font-medium text-gray-300">{label}</label>
                <span className="px-2.5 py-1 text-sm font-semibold text-white bg-gray-700 rounded-md">
                    {value.toFixed(unit === '%' ? 1 : (unit === ' Players' ? 0 : 2))}
                    {unit}
                </span>
            </div>
            <input
                type="range"
                min={min}
                max={max}
                step={step}
                value={value}
                onChange={(e) => setValue(parseFloat(e.target.value))}
                className="w-full h-2 bg-gray-600 rounded-lg appearance-none cursor-pointer range-thumb"
                disabled={disabled}
            />
            <style>{`
                .range-thumb::-webkit-slider-thumb {
                    -webkit-appearance: none; appearance: none;
                    width: 20px; height: 20px;
                    background: ${disabled ? '#6b7280' : '#4f46e5'};
                    cursor: ${disabled ? 'not-allowed' : 'pointer'};
                    border-radius: 50%; border: 2px solid white;
                }
                .range-thumb::-moz-range-thumb {
                    width: 20px; height: 20px;
                    background: ${disabled ? '#6b7280' : '#4f46e5'};
                    cursor: ${disabled ? 'not-allowed' : 'pointer'};
                    border-radius: 50%; border: 2px solid white;
                }
            `}</style>
        </div>
    );
};

// Helper component for the position selection buttons
const PositionSelector = ({ title, positions, selected, onSelect, disabled = false }) => {
    const gridCols = positions.length <= 4 ? `grid-cols-${positions.length}` : 'grid-cols-4';
    return (
        <div className={`bg-gray-800 border border-gray-700 rounded-xl shadow-lg p-4 ${disabled ? 'opacity-50' : ''}`}>
            <h3 className="text-md font-semibold text-center text-indigo-300 mb-3">{title}</h3>
            <div className={`grid ${gridCols} sm:grid-cols-4 gap-2`}>
                {positions.map((pos) => (
                    <button
                        key={pos}
                        onClick={() => onSelect(pos)}
                        disabled={disabled}
                        className={`py-2 px-1 text-xs sm:text-sm font-semibold rounded-lg transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-indigo-400
                        ${selected === pos 
                            ? 'bg-indigo-600 text-white shadow-md' 
                            : 'bg-gray-700 text-gray-300 hover:bg-gray-600'
                        } ${disabled ? 'cursor-not-allowed' : ''}`}
                    >
                        {pos}
                    </button>
                ))}
            </div>
        </div>
    );
};

// Main Application Component
export default function App() {
    // --- STATE MANAGEMENT ---
    const [stack, setStack] = useState(20);
    const [raiseSize, setRaiseSize] = useState(2.2);
    const [rfi, setRfi] = useState(30);
    const [callingRange, setCallingRange] = useState(8);
    const [equity, setEquity] = useState(51);
    const [equityVsColdCaller, setEquityVsColdCaller] = useState(35);
    const [targetEv, setTargetEv] = useState(0.0);
    const [calcMode, setCalcMode] = useState('ev');
    const [rfiPosition, setRfiPosition] = useState('HJ');
    const [heroPosition, setHeroPosition] = useState('BB');
    const [playerCount, setPlayerCount] = useState(9);
    const [antePercentage, setAntePercentage] = useState(12.5);

    // --- DERIVED STATE FOR BREAKDOWN ---
    const [resultValue, setResultValue] = useState(0);
    const [potOnFold, setPotOnFold] = useState(0);
    const [playersToAct, setPlayersToAct] = useState(0);
    const [probColdCall, setProbColdCall] = useState(0);
    const [totalFoldProb, setTotalFoldProb] = useState(0);
    const [bustoutProb, setBustoutProb] = useState(0);

    const POSITION_ORDER = ['UTG', 'UTG+1', 'MP', 'LJ', 'HJ', 'CO', 'BTN', 'SB', 'BB'];
    
    // --- DYNAMIC POSITION LOGIC ---
    const tablePositions = useMemo(() => {
        const numToRemove = 9 - playerCount;
        return POSITION_ORDER.slice(numToRemove);
    }, [playerCount]);

    const availableRfiPos = useMemo(() => {
        if (playerCount === 2) return ['SB'];
        return tablePositions.filter(p => p !== 'BB');
    }, [tablePositions, playerCount]);

    const availableHeroPos = useMemo(() => {
        const rfiIdx = tablePositions.indexOf(rfiPosition);
        if (rfiIdx === -1) return [];
        return tablePositions.slice(rfiIdx + 1);
    }, [rfiPosition, tablePositions]);

    useEffect(() => {
        if (!availableRfiPos.includes(rfiPosition)) {
            setRfiPosition(availableRfiPos[availableRfiPos.length - 2] || availableRfiPos[0]);
        }
        if (!availableHeroPos.includes(heroPosition)) {
            setHeroPosition(availableHeroPos[0]);
        }
    }, [playerCount, rfiPosition, heroPosition, availableRfiPos, availableHeroPos]);

    const handleRfiSelect = (pos) => setRfiPosition(prev => prev === pos ? null : pos);
    const handleHeroSelect = (pos) => setHeroPosition(prev => prev === pos ? null : pos);

    // --- CALCULATION LOGIC ---
    useEffect(() => {
        if (!heroPosition || !rfiPosition) return;

        const effectiveCallingRange = Math.min(rfi, callingRange);
        if (callingRange > rfi) setCallingRange(rfi);

        const heroIdx = tablePositions.indexOf(heroPosition);
        const bbIdx = tablePositions.indexOf('BB');
        const numPlayersToAct = heroIdx !== -1 && bbIdx !== -1 && heroIdx < bbIdx ? bbIdx - heroIdx : 0;
        setPlayersToAct(numPlayersToAct);
        
        const probSingleColdCall = 0.03;
        const pColdCall = 1 - Math.pow(1 - probSingleColdCall, numPlayersToAct);
        const pNoColdCall = 1 - pColdCall;
        setProbColdCall(pColdCall);

        const rfiDecimal = rfi / 100;
        const callingRangeDecimal = effectiveCallingRange / 100;
        const equityDecimal = equity / 100;
        const equityVsColdCallerDecimal = equityVsColdCaller / 100;
        const totalAntes = (antePercentage / 100) * playerCount;

        let deadBlinds = 0;
        if (rfiPosition !== 'SB' && heroPosition !== 'SB') deadBlinds += 0.5;
        if (heroPosition !== 'BB') deadBlinds += 1.0;

        const pRaiserFolds = rfiDecimal > 0 ? (rfiDecimal - callingRangeDecimal) / rfiDecimal : 1;
        const pRaiserCalls = rfiDecimal > 0 ? callingRangeDecimal / rfiDecimal : 0;

        const pTotalFold = pNoColdCall * pRaiserFolds;
        setTotalFoldProb(pTotalFold);
        
        const pBustout = (pNoColdCall * pRaiserCalls * (1 - equityDecimal)) + (pColdCall * (1 - equityVsColdCallerDecimal));
        setBustoutProb(pBustout);

        const currentPotOnFold = raiseSize + totalAntes + deadBlinds;
        setPotOnFold(currentPotOnFold);
        
        const totalPotOnRaiserCall = (stack * 2) + totalAntes + deadBlinds;
        const totalPotOnColdCall = (stack * 2) + raiseSize + totalAntes;

        const ev_Fold = currentPotOnFold;
        
        if (calcMode === 'ev') {
            const ev_RaiserCall = (equityDecimal * totalPotOnRaiserCall) - stack;
            const ev_ColdCall = (equityVsColdCallerDecimal * totalPotOnColdCall) - stack;
            const totalEv = (pNoColdCall * pRaiserFolds * ev_Fold) + (pNoColdCall * pRaiserCalls * ev_RaiserCall) + (pColdCall * ev_ColdCall);
            setResultValue(totalEv);
        } else {
            const ev_ColdCall = (equityVsColdCallerDecimal * totalPotOnColdCall) - stack;
            const term1 = targetEv - (pNoColdCall * pRaiserFolds * ev_Fold) - (pColdCall * ev_ColdCall);
            const term2 = pNoColdCall * pRaiserCalls;
            let minEquity = 0;
            if (term2 > 0 && totalPotOnRaiserCall > 0) {
                const numerator = (term1 / term2) + stack;
                minEquity = numerator / totalPotOnRaiserCall;
            }
            setResultValue(minEquity * 100);
        }

    }, [stack, raiseSize, rfi, callingRange, equity, equityVsColdCaller, calcMode, targetEv, rfiPosition, heroPosition, playerCount, antePercentage, tablePositions]);

    const isEvMode = calcMode === 'ev';
    const isProfitable = isEvMode && resultValue > 0;

    const resultBgColor = useMemo(() => isProfitable ? 'bg-green-500/20 border-green-500' : (isEvMode ? 'bg-red-500/20 border-red-500' : 'bg-blue-500/20 border-blue-500'), [isProfitable, isEvMode]);
    const resultTextColor = useMemo(() => isProfitable ? 'text-green-400' : (isEvMode ? 'text-red-400' : 'text-blue-400'), [isProfitable, isEvMode]);

    const handleScrollToInstructions = (e) => {
        e.preventDefault();
        document.getElementById('instructions').scrollIntoView({ behavior: 'smooth' });
    };

    return (
        <div className="bg-gray-900 text-white min-h-screen font-sans p-4" style={{scrollPaddingTop: '20px'}}>
            <div className="w-full max-w-4xl mx-auto">
                <div className="text-center mb-8 relative">
                    <h1 className="text-4xl font-bold text-indigo-400">3-Bet Shove EV Calculator</h1>
                    <p className="text-gray-400 mt-2">Analyze shove profitability or find your minimum required equity, accounting for simplified cold call risk.</p>
                     <a href="#instructions" onClick={handleScrollToInstructions} className="absolute top-0 right-0 mt-2 text-sm text-indigo-400 hover:text-indigo-300 transition-colors">
                        How to Use
                    </a>
                </div>

                <div className="grid grid-cols-1 md:grid-cols-2 gap-4 mb-8">
                    <PositionSelector title="Villain's Position (RFI)" positions={availableRfiPos} selected={rfiPosition} onSelect={handleRfiSelect} />
                    <PositionSelector title="Your Position" positions={availableHeroPos} selected={heroPosition} onSelect={handleHeroSelect} disabled={!rfiPosition} />
                </div>

                <div className="bg-gray-800 border border-gray-700 rounded-xl shadow-2xl p-6 grid grid-cols-1 md:grid-cols-2 gap-8">
                    {/* Parameters Column */}
                    <div className="space-y-6">
                        <div className="flex justify-between items-center bg-gray-900 p-2 rounded-lg">
                             <h2 className="text-xl font-semibold text-white">Parameters</h2>
                             <div className="flex space-x-1 bg-gray-700 p-1 rounded-lg">
                                <button onClick={() => setCalcMode('ev')} className={`px-4 py-1.5 rounded-md text-sm font-semibold transition-colors ${calcMode === 'ev' ? 'bg-indigo-600 text-white' : 'text-gray-300 hover:bg-gray-600'}`}>Calculate EV</button>
                                <button onClick={() => setCalcMode('equity')} className={`px-4 py-1.5 rounded-md text-sm font-semibold transition-colors ${calcMode === 'equity' ? 'bg-indigo-600 text-white' : 'text-gray-300 hover:bg-gray-600'}`}>Min. Equity</button>
                            </div>
                        </div>
                        
                        {isEvMode ? (
                             <InputSlider label="Your Equity vs Call Range (%)" value={equity} setValue={setEquity} min={0} max={100} step={0.5} unit="%" />
                        ) : (
                             <InputSlider label="Target EV (BB)" value={targetEv} setValue={setTargetEv} min={-5} max={10} step={0.1} unit=" BB" />
                        )}

                        <InputSlider label="Equity vs Cold Caller (%)" value={equityVsColdCaller} setValue={setEquityVsColdCaller} min={0} max={100} step={0.5} unit="%" />
                        <InputSlider label="Effective Stack (BB)" value={stack} setValue={setStack} min={5} max={40} step={0.5} unit=" BB" />
                        <InputSlider label="Villain's Raise Size (BB)" value={raiseSize} setValue={setRaiseSize} min={2} max={5} step={0.1} unit=" BB" />
                        <InputSlider label="Villain's RFI (%)" value={rfi} setValue={setRfi} min={2} max={100} step={0.5} unit="%" />
                        <InputSlider label="Villain's Calling Range (%)" value={callingRange} setValue={setCallingRange} min={0} max={rfi} step={0.5} unit="%" />
                        
                        <div className="bg-gray-900 p-3 rounded-lg space-y-4">
                            <h3 className="text-center font-semibold text-gray-300">Ante Structure</h3>
                            <InputSlider label="Players at Table" value={playerCount} setValue={setPlayerCount} min={2} max={9} step={1} unit=" Players" />
                            <InputSlider label="Ante Size (% of BB)" value={antePercentage} setValue={setAntePercentage} min={8} max={15} step={0.5} unit="%" />
                        </div>
                    </div>
                    {/* Results Column */}
                    <div className="flex flex-col justify-between bg-gray-900 p-6 rounded-lg border border-gray-700">
                        <div>
                             <h2 className="text-xl font-semibold text-white border-b border-gray-700 pb-2 mb-4">
                                {isEvMode ? 'Result' : 'Minimum Equity vs Raiser'}
                            </h2>
                            <div className={`text-center p-6 rounded-lg border transition-colors duration-300 ${resultBgColor}`}>
                                <p className={`text-5xl font-bold ${resultTextColor}`}>
                                    {resultValue.toFixed(2)}
                                    <span className="text-3xl ml-1">{isEvMode ? 'BB' : '%'}</span>
                                </p>
                                <p className={`mt-2 text-lg font-semibold ${resultTextColor}`}>
                                    {isEvMode ? (isProfitable ? 'PROFITABLE SHOVE' : 'UNPROFITABLE SHOVE') : 'VS VILLAIN CALLING RANGE'}
                                </p>
                            </div>
                        </div>
                        
                        <div className="mt-6 space-y-3">
                            <h3 className="text-lg font-semibold text-white border-b border-gray-700 pb-2">Risk & Reward Analysis</h3>
                            <div className="flex justify-between text-sm text-gray-400">
                                <span>Total Fold Probability:</span>
                                <span className="font-mono text-white">{(totalFoldProb * 100).toFixed(1)}%</span>
                            </div>
                            <div className="flex justify-between text-sm text-gray-400">
                                <span>Bustout Probability:</span>
                                <span className="font-mono text-red-400">{(bustoutProb * 100).toFixed(1)}%</span>
                            </div>
                             <div className="flex justify-between text-sm text-gray-400">
                                <span>Cold Call Probability:</span>
                                <span className="font-mono text-white">{(probColdCall * 100).toFixed(1)}%</span>
                            </div>
                            <div className="flex justify-between text-sm text-gray-400">
                                <span>Players Left to Act:</span>
                                <span className="font-mono text-white">{playersToAct}</span>
                            </div>
                            <div className="flex justify-between text-sm text-gray-400">
                                <span>Pot Won on Fold:</span>
                                <span className="font-mono text-white">{potOnFold.toFixed(2)} BB</span>
                            </div>
                             <div className="flex justify-between text-sm text-gray-400">
                                <span>Risk:Reward Ratio:</span>
                                <span className="font-mono text-white">{potOnFold > 0 ? (stack / potOnFold).toFixed(2) : '∞'} : 1</span>
                            </div>
                        </div>
                    </div>
                </div>

                <div className="text-center mt-6 text-xs text-gray-500">
                    <p>Calculations assume RFI folds if a player behind calls your shove.</p>
                    <p>Pot size is dynamically calculated based on selected positions and ante structure.</p>
                </div>
                
                {/* Instructions Section */}
                <div id="instructions" className="mt-12 pt-8 border-t border-gray-700">
                    <h2 className="text-3xl font-bold text-center text-indigo-400 mb-6">How to Use the Calculator</h2>
                    <div className="max-w-3xl mx-auto text-gray-300 space-y-8">
                        <div>
                            <h3 className="text-xl font-semibold text-white mb-2">Goal: Answer "Should I Shove?"</h3>
                            <p>This tool helps you figure out if going all-in is a profitable play in the long run. It does this by calculating the <strong className="text-indigo-300">Expected Value (EV)</strong> of the shove in Big Blinds (BBs). A positive EV means the play is profitable; a negative EV means it's a losing play.</p>
                        </div>

                        <div>
                            <h3 className="text-xl font-semibold text-white mb-3">How to Use It: A Step-by-Step Guide</h3>
                            <div className="space-y-4">
                                <div>
                                    <h4 className="font-bold text-gray-100">1. Choose Your Calculation Mode</h4>
                                    <p className="ml-4"><strong>Calculate EV (Default):</strong> For when you have a specific hand and want to see if it's profitable. You'll input your hand's estimated equity.</p>
                                    <p className="ml-4"><strong>Min. Equity:</strong> For finding the *worst* hand you can profitably shove. You'll set a "Target EV" (usually 0 for breakeven), and the calculator will find the minimum equity needed.</p>
                                </div>
                                <div>
                                    <h4 className="font-bold text-gray-100">2. Set Up the Table Situation</h4>
                                    <p className="ml-4"><strong>Ante Structure:</strong> Set the <strong className="text-indigo-300">Players at Table</strong> and the <strong className="text-indigo-300">Ante Size</strong>.</p>
                                    <p className="ml-4"><strong>Villain's Position (RFI):</strong> Click to select the position of the player who made the initial raise.</p>
                                    <p className="ml-4"><strong>Your Position:</strong> Click to select your position. This list automatically updates to only show valid positions *after* the initial raiser.</p>
                                </div>
                                <div>
                                    <h4 className="font-bold text-gray-100">3. Adjust the Parameters</h4>
                                    <p className="ml-4">These sliders are the core of the calculation. Adjust them based on your reads:</p>
                                    <ul className="list-disc list-inside ml-8 space-y-1 mt-2">
                                        <li><strong className="text-indigo-300">Your Equity vs Call Range (%):</strong> (In EV mode) Your hand's equity against the range the opponent will *call* with.</li>
                                        <li><strong className="text-indigo-300">Equity vs Cold Caller (%):</strong> Your equity against a *very strong* hand (like JJ+/AK).</li>
                                        <li><strong className="text-indigo-300">Effective Stack (BB):</strong> The smaller of your stack and the raiser's.</li>
                                        <li><strong className="text-indigo-300">Villain's Raise Size (BB):</strong> How much the opponent raised to.</li>
                                        <li><strong className="text-indigo-300">Villain's RFI (%):</strong> How often this opponent raises from this position.</li>
                                        <li><strong className="text-indigo-300">Villain's Calling Range (%):</strong> What part of their RFI range they'll call your shove with.</li>
                                    </ul>
                                </div>
                                 <div>
                                    <h4 className="font-bold text-gray-100">4. Analyze the Results</h4>
                                     <p className="ml-4">The main display shows the result. Green is a profitable shove. The "Risk & Reward Analysis" gives you deeper insights:</p>
                                      <ul className="list-disc list-inside ml-8 space-y-1 mt-2">
                                        <li><strong className="text-indigo-300">Total Fold Probability:</strong> Your "fold equity." The chance your shove gets through uncontested.</li>
                                        <li><strong className="text-indigo-300">Bustout Probability:</strong> The total chance you get called *and lose* the hand.</li>
                                      </ul>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

            </div>
        </div>
    );
}
