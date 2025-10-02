<script lang="ts">
    // Game state
    type Player = 'X' | 'O';
    type Cell = Player | null;

    let board: Cell[] = Array(9).fill(null);
    let currentPlayer: Player = 'X';
    let winner: Player | 'Draw' | null = null;
    let winningLine: number[] = [];
    let moves = 0;

    const lines: number[][] = [
        [0, 1, 2],[3, 4, 5],[6, 7, 8], // rows
        [0, 3, 6],[1, 4, 7],[2, 5, 8], // cols
        [0, 4, 8],[2, 4, 6]            // diagonals
    ];

    // PUBLIC_INTERFACE
    export function resetGame() {
        /** Resets the game state to start a fresh match. */
        board = Array(9).fill(null);
        currentPlayer = 'X';
        winner = null;
        winningLine = [];
        moves = 0;
    }

    function checkWinner() {
        for (const [a, b, c] of lines) {
            if (board[a] && board[a] === board[b] && board[b] === board[c]) {
                winner = board[a];
                winningLine = [a, b, c];
                return;
            }
        }
        if (moves === 9 && !winner) {
            winner = 'Draw';
        }
    }

    function handleCellClick(index: number) {
        if (board[index] || winner) return; // ignore if filled or game over
        board[index] = currentPlayer;
        moves += 1;

        // Soft pulse on turn change will be handled with CSS class toggle
        checkWinner();
        if (!winner) {
            currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
        }
    }

    function isWinningCell(i: number) {
        return winningLine.includes(i);
    }
</script>

<svelte:head>
    <title>Tic Tac Toe — Ocean Professional</title>
    <meta name="description" content="Play a minimalist two-player Tic Tac Toe with a modern Ocean Professional theme." />
</svelte:head>

<section class="container fade-in">
    <div class="game card scale-in">
        <header class="game__header">
            <div class="title">
                <span class="logo" aria-hidden="true">◻︎</span>
                <h1>Tic Tac Toe</h1>
            </div>

            <div class="status">
                {#if winner === null}
                    <span class="badge turn" data-player={currentPlayer}>
                        Turn: {currentPlayer}
                    </span>
                {:else if winner === 'Draw'}
                    <span class="badge draw">It's a draw</span>
                {:else}
                    <span class="badge win">Winner: {winner}</span>
                {/if}
            </div>
        </header>

        <div class="hr"></div>

        <div class="board-wrapper">
            <div class="board" aria-label="Tic Tac Toe board" role="grid">
                {#each board as cell, i (i)}
                    <button
                        role="gridcell"
                        aria-label={`Cell ${i + 1} ${cell ? `: ${cell}` : ''}`}
                        class="cell {cell ? 'filled' : ''} {winner && isWinningCell(i) ? 'win' : ''}"
                        on:click={() => handleCellClick(i)}
                        disabled={Boolean(cell) || Boolean(winner)}
                    >
                        <span class="mark {cell === 'X' ? 'x' : cell === 'O' ? 'o' : ''}">
                            {cell}
                        </span>
                    </button>
                {/each}
            </div>
        </div>

        <footer class="game__footer">
            <div class="legend">
                <span class="legend__item">
                    <span class="dot dot--x" aria-hidden="true"></span> X
                </span>
                <span class="legend__item">
                    <span class="dot dot--o" aria-hidden="true"></span> O
                </span>
                <span class="legend__item">
                    <span class="dot dot--win" aria-hidden="true"></span> Win
                </span>
            </div>

            <button class="btn secondary" on:click={resetGame} aria-label="Reset game">
                <svg width="16" height="16" viewBox="0 0 24 24" aria-hidden="true">
                    <path fill="currentColor" d="M12 6V3L8 7l4 4V8c2.76 0 5 2.24 5 5a5 5 0 1 1-8.66-3.54l-1.42-1.42A7 7 0 1 0 19 13c0-3.87-3.13-7-7-7z"/>
                </svg>
                Reset
            </button>
        </footer>
    </div>
</section>

<style>
    .game {
        width: 100%;
        max-width: 680px;
        padding: 18px 18px 16px;
        backdrop-filter: saturate(140%) blur(6px);
    }

    .game__header {
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 16px;
    }

    .title {
        display: flex;
        align-items: center;
        gap: 10px;
    }

    .logo {
        display: inline-flex;
        align-items: center;
        justify-content: center;
        width: 28px;
        height: 28px;
        border-radius: 8px;
        background: linear-gradient(180deg, rgba(37,99,235,0.12), rgba(37,99,235,0.18));
        color: var(--primary);
        border: 1px solid rgba(37,99,235,0.25);
        box-shadow: 0 1px 0 rgba(255,255,255,0.5) inset, var(--shadow-sm);
    }

    h1 {
        font-size: 20px;
        letter-spacing: 0.4px;
        margin: 0;
        color: var(--text);
    }

    .status .badge.turn[data-player="X"] {
        background: rgba(37,99,235,0.10);
        border-color: rgba(37,99,235,0.25);
        color: var(--primary);
    }
    .status .badge.turn[data-player="O"] {
        background: rgba(245,158,11,0.10);
        border-color: rgba(245,158,11,0.25);
        color: #b45309;
    }

    .badge.draw {
        background: rgba(17,24,39,0.06);
        border: 1px solid rgba(17,24,39,0.16);
        color: rgba(17,24,39,0.80);
    }

    .badge.win {
        background: rgba(37, 211, 102, 0.12);
        border: 1px solid rgba(16,185,129,0.35);
        color: #0f766e;
    }

    .board-wrapper {
        margin: 16px 2px 14px;
        padding: 10px;
        background: linear-gradient(180deg, rgba(17,24,39,0.04), rgba(17,24,39,0.02));
        border-radius: 16px;
        border: 1px solid rgba(17,24,39,0.06);
    }

    .board {
        display: grid;
        grid-template-columns: repeat(3, minmax(64px, 1fr));
        gap: 10px;
    }

    @media (min-width: 640px) {
        .board {
            gap: 12px;
        }
    }

    .cell {
        width: var(--cell-size-mobile);
        height: var(--cell-size-mobile);
        border-radius: 16px;
        border: 1px solid rgba(17,24,39,0.08);
        background: var(--surface);
        box-shadow: var(--shadow-md);
        display: grid;
        place-items: center;
        cursor: pointer;
        transition: transform 160ms ease, box-shadow 160ms ease, border-color 160ms ease, background 160ms ease;
        position: relative;
        overflow: hidden;
    }

    @media (min-width: 640px) {
        .cell {
            width: var(--cell-size-desktop);
            height: var(--cell-size-desktop);
            border-radius: 18px;
        }
    }

    .cell::after {
        content: "";
        position: absolute;
        inset: 0;
        background: radial-gradient(240px 120px at 50% -40%, rgba(37,99,235,0.08), transparent 60%);
        opacity: 0;
        transition: opacity 160ms ease;
        pointer-events: none;
    }

    .cell:hover:not(:disabled) {
        transform: translateY(-2px);
        box-shadow: 0 10px 24px rgba(17,24,39,0.10), var(--shadow-sm);
        border-color: rgba(37,99,235,0.25);
    }

    .cell:hover:not(:disabled)::after {
        opacity: 1;
    }

    .cell:disabled {
        cursor: default;
        opacity: 0.98;
    }

    .cell.filled .mark {
        transform: scale(1);
        opacity: 1;
    }

    .cell.win {
        border-color: rgba(245,158,11,0.55);
        background: linear-gradient(180deg, rgba(245,158,11,0.10), rgba(245,158,11,0.14));
        box-shadow: 0 8px 20px rgba(245,158,11,0.24);
        animation: glow 1200ms ease-in-out infinite alternate;
    }

    @keyframes glow {
        from { box-shadow: 0 8px 20px rgba(245,158,11,0.20); }
        to   { box-shadow: 0 10px 28px rgba(245,158,11,0.30); }
    }

    .mark {
        font-size: 36px;
        font-weight: 800;
        letter-spacing: 1px;
        color: var(--text);
        transform: scale(0.9);
        opacity: 0;
        transition: transform 180ms ease, opacity 180ms ease, color 180ms ease;
        text-shadow: 0 1px 0 rgba(255,255,255,0.6);
    }

    @media (min-width: 640px) {
        .mark {
            font-size: 44px;
        }
    }

    .mark.x {
        color: var(--primary);
        text-shadow: 0 6px 18px rgba(37, 99, 235, 0.25);
    }

    .mark.o {
        color: #b45309; /* deeper amber */
        text-shadow: 0 6px 18px rgba(245, 158, 11, 0.25);
    }

    .game__footer {
        margin-top: 6px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 12px;
        flex-wrap: wrap;
    }

    .legend {
        display: inline-flex;
        align-items: center;
        gap: 12px;
        color: rgba(17,24,39,0.75);
        font-size: 14px;
    }
    .legend__item {
        display: inline-flex;
        align-items: center;
        gap: 6px;
    }

    .dot {
        width: 10px;
        height: 10px;
        border-radius: 50%;
        display: inline-block;
        box-shadow: 0 1px 0 rgba(255,255,255,0.5) inset, var(--shadow-sm);
        border: 1px solid rgba(17,24,39,0.12);
    }
    .dot--x { background: rgba(37,99,235,0.85); border-color: rgba(37,99,235,0.35); }
    .dot--o { background: rgba(245,158,11,0.90); border-color: rgba(245,158,11,0.40); }
    .dot--win { background: rgba(16,185,129,0.90); border-color: rgba(16,185,129,0.40); }
</style>
