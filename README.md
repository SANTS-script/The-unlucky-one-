<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>the unlucky one</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            height: 100vh;
            background: linear-gradient(135deg, #090d16, #111827, #1f1224);
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            overflow: hidden;
            color: #f3f4f6;
            padding: 20px;
        }

        header {
            text-align: center;
            margin-bottom: 50px;
            pointer-events: none;
        }

        header h1 {
            font-size: 1.8rem;
            font-weight: 400;
            letter-spacing: 2px;
            color: #f43f5e;
            text-transform: uppercase;
            text-shadow: 0 0 15px rgba(244, 63, 94, 0.3);
            margin-bottom: 8px;
        }

        header p {
            color: #6b7280;
            font-size: 0.9rem;
            font-style: italic;
        }

        .stage {
            perspective: 600px;
            display: flex;
            justify-content: center;
            align-items: center;
            width: 100%;
            height: 150px;
        }

        .envelope-btn {
            position: relative;
            width: 140px;
            height: 95px;
            background-color: #374151;
            border-radius: 4px;
            cursor: pointer;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.6), 0 0 1px rgba(244, 63, 94, 0.2);
            transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1), box-shadow 0.3s ease;
            border: none;
            outline: none;
            touch-action: manipulation;
            -webkit-tap-highlight-color: transparent;
        }

        .envelope-btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            border-left: 70px solid transparent;
            border-right: 70px solid transparent;
            border-top: 55px solid #4b5563;
            transform-origin: top;
            transition: transform 0.4s ease;
            z-index: 5;
        }

        .envelope-btn::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            border-left: 70px solid #1f2937;
            border-right: 70px solid #1f2937;
            border-bottom: 50px solid #1f2937;
            border-radius: 0 0 4px 4px;
            z-index: 4;
        }

        .heart-seal {
            position: absolute;
            top: 45%;
            left: 50%;
            transform: translate(-50%, -50%);
            color: #f43f5e;
            font-size: 1.5rem;
            z-index: 6;
            pointer-events: none;
            transition: transform 0.3s ease;
        }

        .envelope-btn:hover {
            transform: translateY(-8px) scale(1.03);
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.7), 0 0 15px rgba(244, 63, 94, 0.15);
        }

        .envelope-btn:hover .heart-seal {
            transform: translate(-50%, -50%) scale(1.1);
        }

        .envelope-btn:active {
            transform: translateY(2px) scale(0.98);
        }

        .modal {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%) scale(0.8);
            width: 85%;
            max-width: 420px;
            background: #111827;
            border: 1px solid #374151;
            padding: 35px 25px;
            border-radius: 16px;
            box-shadow: 0 25px 60px rgba(0,0,0,0.8);
            text-align: center;
            opacity: 0;
            visibility: hidden;
            transition: all 0.4s cubic-bezier(0.165, 0.84, 0.44, 1);
            z-index: 100;
        }

        .modal.active {
            transform: translate(-50%, -50%) scale(1);
            opacity: 1;
            visibility: visible;
        }

        .category-tag {
            display: inline-block;
            padding: 4px 14px;
            border-radius: 4px;
            font-size: 0.7rem;
            text-transform: uppercase;
            font-weight: bold;
            letter-spacing: 1.5px;
            margin-bottom: 25px;
            border: 1px solid transparent;
        }

        .tag-whatif { color: #f43f5e; background: rgba(244, 63, 94, 0.1); border-color: rgba(244, 63, 94, 0.2); }
        .tag-poem { color: #a855f7; background: rgba(168, 85, 247, 0.1); border-color: rgba(168, 85, 247, 0.2); }
        .tag-message { color: #3b82f6; background: rgba(59, 130, 246, 0.1); border-color: rgba(59, 130, 246, 0.2); }
        .tag-realtalk { color: #f97316; background: rgba(249, 115, 22, 0.1); border-color: rgba(249, 115, 22, 0.2); }

        .message-text {
            font-size: 1.1rem;
            color: #e5e7eb;
            font-weight: 400;
            line-height: 1.6;
            margin-bottom: 30px;
            white-space: pre-line;
        }

        .close-btn {
            background: transparent;
            color: #9ca3af;
            border: 1px solid #374151;
            padding: 10px 24px;
            border-radius: 8px;
            font-size: 0.85rem;
            cursor: pointer;
            transition: all 0.2s;
        }

        .close-btn:hover {
            color: #fff;
            border-color: #f43f5e;
            background: rgba(244, 63, 94, 0.05);
        }

        .overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(3, 7, 18, 0.8);
            backdrop-filter: blur(4px);
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.3s ease;
            z-index: 99;
        }

        .overlay.active {
            opacity: 1;
            visibility: visible;
        }
    </style>
</head>
<body>

    <header>
        <h1>Unsent Notes</h1>
        <p>Tap the seal to read what hurts</p>
    </header>

    <div class="stage">
        <button class="envelope-btn" id="envelope" aria-label="Open letter">
            <div class="heart-seal">💔</div>
        </button>
    </div>

    <div class="overlay" id="overlay" onclick="closeModal()"></div>
    <div class="modal" id="modal">
        <div class="category-tag" id="modal-tag">Category</div>
        <div class="message-text" id="modal-text">"..."</div>
        <button class="close-btn" onclick="closeModal()">Close</button>
    </div>

    <script>
        const pool = {
            whatif: [
                "What if you were ready to give them the world, but they were just looking for an exit?",
                "What if they never actually liked you, but were just lonely while waiting for the one they actually wanted?",
                "What if your only purpose in their life was to heal them enough so they could be perfect for someone else?",
                "What if you never stop comparing everyone else to the person who broke you?",
                "What if they remember you, but only as an embarrassing phase or a temporary distraction?",
                "What if you cross their mind sometimes and they're just glad they walked away?",
                "What if you spend your whole life trying to replace someone who forgot your name within a month?",
                "What if you were the right person, but they just didn't love you enough to try?",
                "What if they finally learn how to love properly, but on the person who comes after you?",
                "What if they never regret losing you?"
            ],
            poem: [
                "I tore myself to pieces\nTo keep you warm and whole,\nBut you left me in the winter\nWith a frozen, hollow soul.",
                "You became a habit\nI had to break,\nA beautiful dream\nTurned wide awake.\nI loved the version\nI made of you,\nIt hurts to admit\nNone of it was true.",
                "I was a chapter,\nYou were the book.\nI gave you my everything,\nYou took what you took.",
                "I stayed up waiting for a spark\nWhile you were sleeping in the dark.\nI burned my hands to keep you bright;\nYou left when you no longer needed light.",
                "You look at me and see a friend,\nA winding road without an end.\nI look at you and see the sun,\nAnd realize I am no one.",
                "I kept the pieces you threw away,\nAnd tried to build a yesterday.\nBut dust is dust and bone is bone,\nAnd in the end, I sit alone.",
                "It didn't end with screams or cries,\nJust fading sparks and quiet sighs.\nYou didn't block or lock the door,\nYou simply didn't care no more."
            ],
            message: [
                "You keep keeping your door open for someone who doesn't even want to walk up your driveway. Close it.",
                "Stop checking your phone. If they wanted to talk to you, they would. Silence is an answer.",
                "It’s time to accept that you can't love someone into loving you back. Some things are just beautiful dead ends.",
                "You shouldn't have to beg someone to notice the effort you're making. Love is a partnership, not an audition.",
                "The hardest part isn't letting go, it's realizing there was never really anything to hold on to in the first place.",
                "They didn't give you closure because they didn't care enough to watch you heal. Let that be your closure.",
                "You are drowning yourself trying to save someone who isn't even in the water.",
                "Stop looking for their face in crowded rooms. They aren't looking for yours.",
                "You are holding onto an apology that will never be sent. Forgive yourself for waiting.",
                "They proved to you a thousand times who they were, but you chose to believe your own imagination instead."
            ],
            realtalk: [
                "Real talk: They aren't confused. They just don't want you. Stop making excuses for their lack of interest.",
                "You’re not unlucky in love. You’re just addicted to trying to fix people who see you as disposable.",
                "They didn't break your heart, they broke your ego. Your heart is completely fine, you're just mad you weren't chosen.",
                "Stop romanticizing the bare minimum just because you're starving for affection.",
                "They can go days without talking to you because you're an option, not a priority.",
                "You lost someone who didn't care about losing you. They lost someone who would have given them everything.",
                "If someone wants you in their life, they will put you there. You won't have to fight or beg for a spot.",
                "They treat you like a backup plan because you keep showing up every time they fail with someone else.",
                "Stop breaking your own heart trying to make a relationship work that only exists in your head.",
                "You aren't hard to love. You’re just trying to squeeze milk from a stone."
            ]
        };

        const categories = ['whatif', 'poem', 'message', 'realtalk'];
        const envelope = document.getElementById('envelope');

        envelope.addEventListener('click', () => {
            openEnvelope();
        });

        function openEnvelope() {
            const randomCat = categories[Math.floor(Math.random() * categories.length)];
            const messages = pool[randomCat];
            const randomMessage = messages[Math.floor(Math.random() * messages.length)];

            showModal(randomCat, randomMessage);
        }

        function showModal(category, text) {
            const modal = document.getElementById('modal');
            const overlay = document.getElementById('overlay');
            const tag = document.getElementById('modal-tag');
            const content = document.getElementById('modal-text');

            tag.className = `category-tag tag-${category}`;
            if(category === 'whatif') tag.innerText = 'What-If';
            if(category === 'poem') tag.innerText = 'Poem';
            if(category === 'message') tag.innerText = 'Note to Self';
            if(category === 'realtalk') tag.innerText = 'Real Talk';
            
            content.innerText = text;

            modal.classList.add('active');
            overlay.classList.add('active');
        }

        function closeModal() {
            document.getElementById('modal').classList.remove('active');
            document.getElementById('overlay').classList.remove('active');
        }
    </script>
</body>
</html>
