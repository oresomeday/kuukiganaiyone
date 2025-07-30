<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>自転車に乗る男</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            text-align: center;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            margin: 0;
            padding: 5px;
            min-height: 100vh;
            color: white;
            overflow-x: hidden;
            user-select: none;
            -webkit-user-select: none;
            -webkit-touch-callout: none;
        }
        
        .game-container {
            max-width: 100%;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 20px;
            padding: 10px;
            backdrop-filter: blur(10px);
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
        }
        
        h1 {
            font-size: 2em;
            margin-bottom: 20px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
        }
        
        /* ミニゲーム用スタイル */
        .mini-game {
            display: block;
        }
        
        .mini-game.hidden {
            display: none;
        }
        
        .bike-game-area {
            width: 100%;
            height: 300px;
            background: linear-gradient(to bottom, #87CEEB 0%, #98FB98 100%);
            position: relative;
            border: 3px solid #fff;
            border-radius: 15px;
            overflow: hidden;
            margin: 10px 0;
            touch-action: none;
        }
        
        .road {
            position: absolute;
            bottom: 0;
            width: 100%;
            height: 40px;
            background: #555;
            border-top: 3px dashed #fff;
        }
        
        .bike {
            position: absolute;
            bottom: 50px;
            right: 30px;
            font-size: 2em;
            transition: top 0.15s ease;
            z-index: 10;
        }
        
        .obstacle-group {
            position: absolute;
            left: -50px;
            width: 20px;
            height: 260px;
            top: 30px;
        }
        
        .obstacle {
            position: absolute;
            width: 20px;
            background: #DC143C;
            border-radius: 10px;
            border: 2px solid #8B0000;
            box-shadow: 0 0 10px rgba(220, 20, 60, 0.5);
        }
        
        .obstacle.top {
            top: 0;
            height: 70px;
        }
        
        .obstacle.middle {
            top: 100px;
            height: 50px;
        }
        
        .obstacle.bottom {
            bottom: 0;
            height: 70px;
        }
        
        .gap-indicator {
            position: absolute;
            width: 25px;
            height: 50px;
            background: rgba(0, 255, 0, 0.3);
            border: 2px dashed #00FF00;
            border-radius: 10px;
            left: -2px;
        }
        
        .touch-controls {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin: 10px 0;
        }
        
        .touch-button {
            background: linear-gradient(45deg, #4CAF50, #45a049);
            color: white;
            border: none;
            padding: 15px;
            font-size: 1.8em;
            border-radius: 50%;
            cursor: pointer;
            width: 60px;
            height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.2s ease;
            user-select: none;
            -webkit-user-select: none;
            touch-action: manipulation;
        }
        
        .touch-button:active {
            transform: scale(0.9);
            background: linear-gradient(45deg, #45a049, #4CAF50);
        }
        
        .control-button {
            background: linear-gradient(45deg, #4CAF50, #45a049);
            color: white;
            border: none;
            padding: 12px 25px;
            font-size: 1.1em;
            border-radius: 25px;
            cursor: pointer;
            margin: 10px;
            transition: all 0.3s ease;
            touch-action: manipulation;
        }
        
        .control-button:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
        }
        
        .retry-button {
            background: linear-gradient(45deg, #ff6b6b, #ee5a24);
            color: white;
            border: none;
            padding: 12px 25px;
            font-size: 1.1em;
            border-radius: 25px;
            cursor: pointer;
            margin: 10px;
            transition: all 0.3s ease;
            touch-action: manipulation;
        }
        
        /* スロットゲーム用スタイル */
        .slot-game {
            display: none;
        }
        
        .slot-game.show {
            display: block;
        }
        
        .slot-machine {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin: 20px 0;
            flex-wrap: wrap;
        }
        
        .reel-container {
            width: 80px;
            height: 80px;
            border: 4px solid #fff;
            border-radius: 15px;
            overflow: hidden;
            position: relative;
            background: linear-gradient(45deg, #ff6b6b, #feca57);
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
        }
        
        .reel {
            position: absolute;
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1em;
            font-weight: bold;
            color: #333;
            transition: transform 0.1s linear;
        }
        
        .spin-button {
            background: linear-gradient(45deg, #ff6b6b, #ee5a24);
            color: white;
            border: none;
            padding: 12px 25px;
            font-size: 1.2em;
            border-radius: 50px;
            cursor: pointer;
            margin: 15px;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
            touch-action: manipulation;
        }
        
        .spin-button:hover {
            (-2px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.4);
        }
        
        .spin-button:disabled {
            background: #999;
            cursor: not-allowed;
            transform: none;
        }
        
        .result {
            margin: 15px 0;
            font-size: 1em;
            min-height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 10px;
            padding: 10px;
        }
        
        .sentence {
            font-size: 1.3em;
            color: #feca57;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        }
        
        .remaining-turns {
            font-size: 1em;
            margin: 10px 0;
            background: rgba(255, 255, 255, 0.3);
            padding: 8px 16px;
            border-radius: 20px;
            display: inline-block;
        }
        
        /* 紙吹雪エフェクト */
        .confetti-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 999;
            display: none;
        }
        
        .confetti-overlay.show {
            display: block;
        }
        
        .confetti-piece {
            position: absolute;
            font-size: 2em;
            animation: confettiFall 3s linear;
            pointer-events: none;
        }
        
        @keyframes confettiFall {
            0% { 
                (-100vh) rotate(0deg);
                opacity: 1;
            }
            100% { 
                (100vh) rotate(360deg);
                opacity: 0;
            }
        }
        
        .ending-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #ff6b6b 0%, #feca57 25%, #48c6ef 50%, #ff6b6b 75%, #feca57 100%);
            background-size: 400% 400%;
            animation: gradientShift 3s ease infinite;
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            overflow-y: auto;
            padding: 20px;
        }
        
        .ending-screen.show {
            display: flex;
        }
        
        @keyframes gradientShift {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }
        
        .ending-message {
            font-size: 2em;
            font-weight: bold;
            text-shadow: 3px 3px 6px rgba(0, 0, 0, 0.5);
            margin-bottom: 20px;
            text-align: center;
            line-height: 1.4;
            max-width: 90%;
            background: rgba(255, 255, 255, 0.1);
            padding: 20px;
            border-radius: 15px;
            backdrop-filter: blur(10px);
            white-space: pre-line;
            /* 自動スクロール用 */
            animation: scrollUp 20s linear infinite;
            transform: translateY(100);
        }
        
        /* 自動スクロールアニメーション */
        @keyframes scrollUp {
            0% {
                transform: translateY(100);
            }
            100% {
                transform: translateY(-100%);
            }
        }
        
        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
            40% { transform: translateY(-10px); }
            60% { transform: translateY(-5px); }
        }
        
        /* スマホ対応 */
        @media (max-width: 768px) {
            .game-container { padding: 5px; }
            .bike-game-area { height: 250px; margin: 5px 0; }
            .bike { font-size: 1.8em; bottom: 45px; right: 25px; }
            .touch-button { width: 50px; height: 50px; font-size: 1.5em; }
            .ending-message {
                font-size: 1.5em;
                padding: 15px;
                animation: scrollUp 25s linear infinite;
            }
            .reel-container { width: 70px; height: 70px; }
            .reel { font-size: 0.9em; }
            .obstacle-group { height: 210px; top: 30px; }
            .obstacle.top { height: 50px; }
            .obstacle.middle { height: 35px; top: 75px; }
            .obstacle.bottom { height: 50px; bottom: 0; }
            .gap-indicator { height: 35px; }
        }
        
        @media (max-width: 480px) {
            body { padding: 2px; }
            .game-container { padding: 3px; border-radius: 10px; }
            .bike-game-area { height: 220px; }
            .ending-message { 
                font-size: 1.2em;
                animation: scrollUp 30s linear infinite;
            }
            h1 { font-size: 1.5em; margin-bottom: 10px; }
            .obstacle-group { height: 180px; top: 30px; }
            .obstacle.top { height: 45px; }
            .obstacle.middle { height: 30px; top: 70px; }
            .obstacle.bottom { height: 45px; bottom: 0; }
        }
    </style>
</head>
<body>
    <div class="game-container">
        <h1>🚴‍♂️ 自転車に乗る男</h1>
        
        <!-- ミニゲーム部分 -->
        <div class="mini-game" id="miniGame">
            <div class="bike-game-area" id="gameArea">
                <div class="road"></div>
                <div class="bike" id="bike">🚴‍♂️</div>
            </div>
            
            <div class="touch-controls">
                <button class="touch-button" id="upButton">↑</button>
                <button class="touch-button" id="downButton">↓</button>
            </div>
            
            <div class="game-controls">
                <button class="control-button" id="startButton" onclick="startBikeGame()">スタート</button>
            </div>
        </div>
        
        <!-- スロットゲーム部分 -->
        <div class="slot-game" id="slotGame">
            <div class="slot-machine">
                <div class="reel-container">
                    <div class="reel" id="reel1">空気</div>
                </div>
                <div class="reel-container">
                    <div class="reel" id="reel2">が</div>
                </div>
                <div class="reel-container">
                    <div class="reel" id="reel3">ない</div>
                </div>
            </div>
            
            <button class="spin-button" id="spinButton" onclick="spin()">スピン！</button>
            
            <div class="remaining-turns">残り回数: <span id="remainingTurns">0</span></div>
            
            <div class="result" id="result">
                <div class="sentence" id="sentence">空気がない</div>
            </div>
            
                        <button class="retry-button" onclick="restartGame()" id="retryButton">最初からやり直す</button>
        </div>
    </div>

    <!-- 紙吹雪オーバーレイ -->
    <div class="confetti-overlay" id="confettiOverlay"></div>

    <!-- エンディング画面 -->
    <div class="ending-screen" id="endingScreen">
        <div class="ending-message" id="endingMessage">
            <!-- ここにA,B,C,D,Eのメッセージがランダムで表示されます -->
        </div>
        
        <button class="retry-button" onclick="restartFromEnding()">最初からやり直す</button>
    </div>

    <script>
        // ミニゲーム関連の変数
        let bikeGameRunning = false;
        let bikePosition = 1; // 0=上、1=中、2=下
        let obstacleGroups = [];
        let passCount = 0;
        let earnedTurns = 0;
        let gameSpeed = 3; // 初期スピード
        let speedLevel = 1;
        let obstacleSpawnTimer = 0;
        
        // スロットゲーム関連の変数
        const reel1Words = ['空気'];
        const reel2Words = ['が', 'は', 'も'];
        const reel3Words = ['ある', 'ない'];
        let isSpinning = false;
        let remainingTurns = 0;
        
        // エンディングメッセージ（後で編集用）
        const endingMessages = {
            A: `
            「空気がない」

放課後の夕焼けは、A木山の背中をやさしく押していた。

「今日も平和だったなあ」

ひとり言ちつつ、彼はいつものように愛車「雷神号」にまたがり、家路をたどっていた。雷神号はママチャリとは呼ばせない。前カゴのさび、キーキー鳴るブレーキ音、そしてひび割れたサドルのビニールが、戦士の風格を漂わせていた。

「明日、テストだったよな…ま、なんとかなるか」

そんな軽口を叩きながら、彼は坂道を下っていた。風を切る心地よさ。車輪のリズムに合わせて脳内に流れるBGM。全てが青春だった。

しかし、悲劇はその時、訪れた。

「おっと、ポール！」

歩道と車道の境界に立つ、あの忌々しい鉄製のポールが、まるでカモフラージュでもしているかのように、視界の中に突然現れた。避けるには、微妙な距離だった。

A木山の判断は早かった。

「イケるッ！」

ハンドルを右に切る。

だが、雷神号の前輪は意思とは裏腹にポールを直撃。ゴンッという鉄と鉄のやり取りのあと、彼の身体は空中を舞い、アスファルトに華麗な背面着地を決めた。

沈黙が訪れた。

道行く人もいない。自販機だけがウィーンと低い機械音を鳴らしている。

その中で、A木山はゆっくりと口を開いた。

「く……空気が……ない……」

そう言い残し、彼は静かに目を閉じた。

本当に空気がなかったのは、自転車の前輪のチューブだったのか、それとも彼の肺だったのか、真相は闇の中である。

だが翌日、彼の姿は教室にはなかった。

黒板には小さく書かれていた。

「A木山、転倒により永眠。空気注入推奨」
`,
            
            B: `「く、空気がない」

放課後。
A木山は、いつものように愛車・雷神号で帰宅の途についた。

雷神号は、父が粗大ゴミから救出してきた“レスキューチャリ”である。サドルはテープまみれ、ブレーキは一か八か、タイヤは慢性的に貧血気味。だが彼にとっては、これが青春の戦車だった。

「今日も無事に一日が終わった…この平穏が、ずっと続けばいいのに」

などとフラグを立てながら、A木山はいつもの坂道にさしかかる。

「シュイーーーン！！」
下り坂、全開。風が気持ちいい。脳内ではロッキーのテーマ。感動のラスト15秒。

が、その時だった。

――ポール。

道の真ん中に、いつもと同じ場所に、いつもと同じ無表情で立っていた。
だが今日だけは違った。A木山の目には、なぜかポールが3本に見えた。

「……増えてない？」

試される反射神経。時間がスローモーションになる。

「左はトラック、右はガードレール…真ん中しかないッ！」

ゴンッ！！！

正面衝突。
自転車が跳ね、A木山も跳ねた。彼の身体は空を舞い、そして地球の引力に負け、地面に抱きしめられた。

静寂。

近くのカラスが、「カァ……（うわ、やったな）」と鳴いた。
遠くで救急車のサイレンが聞こえたが、それは彼のためではなかった。隣町の火事らしい。

A木山は、かすかに口を開いた。

「く……空気が……ない……」

誰に言うでもなく、ただ呟いた。
彼の目は、自転車のタイヤを見つめていた。前輪は、ぐにゃりとつぶれ、完全に空気が抜けていた。

それが、自分の肺と重なるなんて、思いもしなかった。

彼のスマホの万歩計アプリは、最後の記録としてこう表示された。

「今日の走行距離：あと3mで家でした」

――翌日、学校には彼の自転車だけが無言で停められていた。
先生が言った。「この子…本当に“息切れ”しちゃったんだね…」
`,
            
            C: `「く、空気がない〜Last Breath Symphony〜」

A木山は生まれながらにして、何かにぶつかる星の下にいた。

小学生の頃は給食の牛乳パックで前歯を折り、中学ではハトと正面衝突。高校に入ってからも、自動ドアに拒絶されて鼻血を出すなど、世界が彼を拒んでいるとしか思えない人生だった。

それでも彼は今日も自転車に乗っていた。
愛車・雷神号と共に。タイヤはペッタンコ、ギアは「無段階の苦行モード」。空気入れは3年前に捨てた。なぜなら「空気なんて目に見えないし」…という哲学的な理由から。

彼は坂を下っていた。風は背中を押し、脳内ではなぜかベートーヴェン第九が鳴っていた。

その時、彼は見た。
ポール。
いや、“神の使い”とすら思えるほど、堂々と道のど真ん中に鎮座する、あの忌まわしき鉄の柱。

「来たか…運命よ…！」

彼はハンドルを切った。だがタイヤは言った。

「いや無理。だって空気ないもん」

ゴンッ！！

衝突。飛翔。無音。

空中で一回転半。着地は完璧。が、本人は沈黙。

顔は地面にべったり。ランドセル（なぜか背負っていた）がクッションにならず、むしろ首を固定する凶器となっていた。

彼は、アスファルトの冷たさを頬で感じながら、最後の言葉を絞り出した。

「く……空気が……ない……」

彼の目線の先には、雷神号の前輪。べちゃりと潰れたタイヤ。その横に、小さな張り紙が落ちていた。

「パンク修理500円」

彼の魂は、そこからスーッと抜けていった。肺から、じゃなく、タイヤのバルブから。

葬儀には三人しか来なかった。
母、ポール（倒された状態で）、そしてタイヤ修理屋の店主である。

誰かが棺に花を入れようとしたが、母は泣きながらそれを制止した。

「この子、花粉症だったのよ…」

棺の中には、空気入れがそっと添えられた。
もう空気がなくても、大丈夫なように。
`,
            
            D: `「空気の読めない男」

A木山は空気が読めなかった。
それは比喩ではない。本当に「空気」というものに縁がなかった。

小学校では「お前、今その発言は空気読めよ」と言われ、空を仰いで「どこにあるんだよ空気」と真顔で返し、いじめが始まった。

中学では肺活量測定中に倒れ、「吸えてない、吸えてないよ！」と先生にパニックボイスを浴びせられた。

高校ではそのまま“空気の読めない男”という二つ名で都市伝説化。七不思議の第八番に登録された。

そしてその日も、A木山は空気の薄い人生を走っていた。

愛車・雷神号に乗り、帰宅中。タイヤは当然のようにペッタンコ。
空気を入れようとすると、ポンプのバルブが折れて死ぬ呪いがあると信じていた。

彼の人生に、空気は不要だった。

坂道を下る。

今日も夕陽が綺麗だ。彼は思った。

「太陽、ずっと見てたら目潰れるっていうけど、俺にはそんな気配がない。視神経まで空気抜けてんのか？」

その瞬間だった。

視界の端にポール。

あの灰色の憎き棒。
毎朝、毎夕、道の真ん中に立ち、「絶対に通すまい」という意志だけでこの街の交通を統治していたポール様。町内会の神。

「またお前か……」

A木山は呟いた。

ハンドルを切ろうとしたが、空気がない。タイヤはぺしゃんこ。ブレーキ？効かない。
運命？止まらない。

ポールが近づく。いや、彼が近づいてるのだが、心理的にはポールが突進してきていた。
鉄の凶器。無言の殺意。

ドカン。

彼は空へ放り出された。
脳内BGMは突然「千の風になって」に切り替わる。だが風はない。風は、空気からできているからだ。

そして静寂。

アスファルトに沈んだA木山は、目だけを動かして、自転車を見た。
前輪のタイヤは…ぺちゃんこ。
彼の肺も…ぺちゃんこ。

そして、言った。

「く……空気が……ない……」

その言葉と共に、彼の口から、しぼんだ風船のような「プスーーッ」という音が漏れた。
死に際まで空気がなかった。


---

翌朝。

道には彼の自転車が置かれたままだった。誰かがそこに花を手向けていた。
ポールだった。

風にひらめく張り紙。

「空気、足りてますか？」
`,
            
            E: `「無呼吸ヒーロー A木山」

A木山はヒーローだった。

彼の特技は「呼吸を忘れること」。
水泳ではいつも25メートルの途中で意識を失い、救助される。
体育の持久走では、1周目で顔が紫になり、保健室に担がれる。

病名：慢性・空気感欠如症候群（K.Y.S.）
別名、“空気の読めない肺”。

それでも彼は生きていた。誰も望んでいないのに。

そして今日も、彼はボロボロのチャリ・雷神号に乗って帰っていた。
タイヤの空気はゼロ。ブレーキは飾り。サドルはタコ焼きのように潰れていた。

「空気って、結局幻想なんじゃないか？」
彼は空を見上げて言った。肺に空気はないのに、なぜか言葉は出る。不思議。

道は坂道。下り坂。

風は…吹かない。空気がないから。

彼の目に映ったのは、前方のポール。
それはもはや、“神”のように立っていた。無言の裁き。

「いいだろう…」
A木山はペダルを踏み、最後のスピードを出した。

「空気がなくても……走れるって証明してやるッ！！」

瞬間、彼の脳内で花火が打ち上がった。いや、血管が切れただけだった。

ポールが迫る。

ポールは動かない。神だから。

ドガッ！！！！！

そして沈黙。

A木山の体はポールに巻きつくように変形し、あらゆる医学の限界に挑戦する形状で着地した。

最後の力で彼は一言、吐き出す。

「く……空気が……ない……」

その声は、あたりに響いた。誰も聞いていなかったが、Apple WatchのSiriが“緊急通報しますか？”と反応した。


---

【後日談】

ニュース番組で、アナウンサーが語った。

「高校生が転倒し死亡。原因は、タイヤの空気不足と、深刻な“人生の空気不足”と見られています」

そして、画面に映った遺影。
A木山の写真には、なぜか背景にポールが写っていた。


---

【その後】

雷神号は事故現場にそのまま残され、「交通安全の象徴」として塗装され、
今では観光名所となっている。

正式名称は――
『無呼吸の勇者A木山殉職の地 〜エアー・レス・モニュメント〜』

観光案内板にはこう書かれている。

> 「空気は、読めるうちに読みましょう。」
`
        };
        
        // タッチコントロール設定
        function setupTouchControls() {
            const upButton = document.getElementById('upButton');
            const downButton = document.getElementById('downButton');
            
            upButton.addEventListener('touchstart', (e) => {
                e.preventDefault();
                moveUp();
            });
            
            upButton.addEventListener('mousedown', (e) => {
                e.preventDefault();
                moveUp();
            });
            
            downButton.addEventListener('touchstart', (e) => {
                e.preventDefault();
                moveDown();
            });
            
            downButton.addEventListener('mousedown', (e) => {
                e.preventDefault();
                moveDown();
            });
        }
        
        function moveUp() {
            if (bikeGameRunning && bikePosition > 0) {
                bikePosition--;
                updateBikePosition();
            }
        }
        
        function moveDown() {
            if (bikeGameRunning && bikePosition < 2) {
                bikePosition++;
                updateBikePosition();
            }
        }
        
        // 自転車ゲームの開始
        function startBikeGame() {
            if (bikeGameRunning) return;
            
            bikeGameRunning = true;
            obstacleGroups = [];
            passCount = 0;
            earnedTurns = 0;
            bikePosition = 1;
            gameSpeed = 3;
            speedLevel = 1;
            obstacleSpawnTimer = 0;
            
            document.getElementById('startButton').disabled = true;
            document.getElementById('startButton').textContent = 'プレイ中...';
            
            const existingObstacles = document.querySelectorAll('.obstacle-group');
            existingObstacles.forEach(obstacle => obstacle.remove());
            
            document.addEventListener('keydown', handleKeyPress);
            
            updateBikePosition();
            
            requestAnimationFrame(gameLoop);
        }
        
        function handleKeyPress(event) {
            if (!bikeGameRunning) return;
            
            if (event.key === 'ArrowUp') {
                event.preventDefault();
                moveUp();
            } else if (event.key === 'ArrowDown') {
                event.preventDefault();
                moveDown();
            }
        }
        
        function updateBikePosition() {
            const bike = document.getElementById('bike');
            // スマホでの位置調整
            const isMobile = window.innerWidth <= 768;
            const positions = isMobile ? [60, 105, 150] : [70, 125, 180];
            bike.style.top = positions[bikePosition] + 'px';
        }
        
        function createObstacleGroup() {
            const gameArea = document.getElementById('gameArea');
            
            const gapPosition = Math.floor(Math.random() * 3);
            
            const obstacleGroup = document.createElement('div');
            obstacleGroup.className = 'obstacle-group';
            obstacleGroup.style.left = '-50px';
            
            const gapIndicator = document.createElement('div');
            gapIndicator.className = 'gap-indicator';
            
            if (gapPosition === 0) {
                gapIndicator.style.top = '30px';
            } else if (gapPosition === 1) {
                gapIndicator.style.top = '85px';
            } else {
                gapIndicator.style.top = '140px';
            }
            
            obstacleGroup.appendChild(gapIndicator);
            
            for (let i = 0; i < 3; i++) {
                if (i !== gapPosition) {
                    const obstacle = document.createElement('div');
                    obstacle.className = 'obstacle';
                    
                    if (i === 0) {
                        obstacle.classList.add('top');
                    } else if (i === 1) {
                        obstacle.classList.add('middle');
                    } else {
                        obstacle.classList.add('bottom');
                    }
                    
                    obstacleGroup.appendChild(obstacle);
                }
            }
            
            gameArea.appendChild(obstacleGroup);
            
            obstacleGroups.push({
                element: obstacleGroup,
                x: -50,
                gapPosition: gapPosition,
                passed: false
            });
        }
        
        function updateObstacleGroups() {
            const gameAreaWidth = document.getElementById('gameArea').offsetWidth;
            
            obstacleGroups.forEach((group, index) => {
                group.x += gameSpeed;
                group.element.style.left = group.x + 'px';
                
                const bikeX = gameAreaWidth - 55;
                
                if (!group.passed && group.x > bikeX - 20 && group.x < bikeX + 20) {
                    if (bikePosition === group.gapPosition) {
                        group.passed = true;
                        passCount++;
                        earnedTurns++;
                        
                        if (passCount % 3 === 0) {
                            gameSpeed += 0.5;
                            speedLevel++;
                        }
                        
                        const obstacles = group.element.querySelectorAll('.obstacle');
                        obstacles.forEach(obstacle => {
                            obstacle.style.background = '#32CD32';
                            obstacle.style.borderColor = '#228B22';
                        });
                    } else {
                        gameOver();
                        return;
                    }
                }
                
                if (group.x > gameAreaWidth + 100) {
                    group.element.remove();
                    obstacleGroups.splice(index, 1);
                }
            });
        }
        
        function gameOver() {
            bikeGameRunning = false;
            document.removeEventListener('keydown', handleKeyPress);
            
            document.getElementById('startButton').disabled = false;
            document.getElementById('startButton').textContent = 'スタート';
            
            setTimeout(() => {
                switchToSlot();
            }, 1000);
        }
        
        function gameLoop() {
            if (!bikeGameRunning) return;
            
            obstacleSpawnTimer++;
            const spawnInterval = Math.max(80 - speedLevel * 5, 40);
            
            if (obstacleSpawnTimer > spawnInterval) {
                createObstacleGroup();
                obstacleSpawnTimer = 0;
            }
            
            updateObstacleGroups();
            
            requestAnimationFrame(gameLoop);
        }
        
        function switchToSlot() {
            obstacleGroups.forEach(group => {
                group.element.remove();
            });
            obstacleGroups = [];
            
            document.getElementById('miniGame').classList.add('hidden');
            document.getElementById('slotGame').classList.add('show');
            remainingTurns = earnedTurns;
            document.getElementById('remainingTurns').textContent = remainingTurns;
            updateSentence();
        }
        
        function restartGame() {
            document.getElementById('endingScreen').classList.remove('show');
            document.getElementById('confettiOverlay').classList.remove('show');
            
            document.getElementById('slotGame').classList.remove('show');
            
            document.getElementById('miniGame').classList.remove('hidden');
            
            isSpinning = false;
            remainingTurns = 0;
            passCount = 0;
            earnedTurns = 0;
            bikePosition = 1;
            gameSpeed = 3;
            speedLevel = 1;
            obstacleSpawnTimer = 0;
            bikeGameRunning = false;
            
            document.getElementById('remainingTurns').textContent = remainingTurns;
            
            document.getElementById('spinButton').disabled = false;
            
            document.getElementById('startButton').disabled = false;
            document.getElementById('startButton').textContent = 'スタート';
            
            const existingObstacles = document.querySelectorAll('.obstacle-group');
            existingObstacles.forEach(obstacle => obstacle.remove());
            
            updateSentence();
        }
        
        function restartFromEnding() {
            restartGame();
        }
        
        // 紙吹雪エフェクト
        function showConfetti() {
            const overlay = document.getElementById('confettiOverlay');
            overlay.classList.add('show');
            overlay.innerHTML = '';
            
            const confettiTypes = ['🎉', '🎊', '✨', '🌟', '💫', '🎈'];
            
            for (let i = 0; i < 50; i++) {
                setTimeout(() => {
                    const confetti = document.createElement('div');
                    confetti.className = 'confetti-piece';
                    confetti.textContent = confettiTypes[Math.floor(Math.random() * confettiTypes.length)];
                    confetti.style.left = Math.random() * 100 + '%';
                    confetti.style.animationDelay = Math.random() * 2 + 's';
                    confetti.style.animationDuration = (Math.random() * 2 + 2) + 's';
                    overlay.appendChild(confetti);
                    
                    setTimeout(() => {
                        if (confetti.parentNode) {
                            confetti.parentNode.removeChild(confetti);
                        }
                    }, 5000);
                }, i * 50);
            }
        }
        
        // スロットゲーム関数
        function getRandomWord(wordArray) {
            return wordArray[Math.floor(Math.random() * wordArray.length)];
        }
        
        function updateSentence() {
            const word1 = document.getElementById('reel1').textContent;
            const word2 = document.getElementById('reel2').textContent;
            const word3 = document.getElementById('reel3').textContent;
            
            document.getElementById('sentence').textContent = word1 + word2 + word3;
        }
        
        function checkWin(word1, word2, word3) {
            const sentence = word1 + word2 + word3;
            
            if (sentence === '空気がない') {
                // 紙吹雪を表示
                showConfetti();
                
                // 3秒後にエンディングを表示
                setTimeout(() => {
                    showEnding();
                }, 3000);
                
                return -1; // 特別な値でエンディング表示を示す
            }
            
            return 0;
        }
        
        function getEncouragementMessage(word1, word2, word3) {
            const messages = [
                'おしい！もう少し！',
                'あと一歩！',
                'がんばって！',
                'いい感じ！',
                'もう一回！',
                'きっと当たる！',
                'チャンスはまだある！',
                '諦めないで！'
            ];
            
            if (word1 === '空気') {
                return 'おしい！「空気」は合ってる！';
            } else if (word2 === 'が') {
                return 'いいね！「が」は正解！';
            } else if (word3 === 'ない') {
                return 'もう少し！「ない」は当たり！';
            }
            
            return messages[Math.floor(Math.random() * messages.length)];
        }
        
        function showEnding() {
            // ランダムにA,B,C,D,Eのいずれかを選択
            const messageKeys = ['A', 'B', 'C', 'D', 'E'];
            const randomKey = messageKeys[Math.floor(Math.random() * messageKeys.length)];
            
            document.getElementById('endingMessage').textContent = endingMessages[randomKey];
            document.getElementById('endingScreen').classList.add('show');
        }
        
        function animateReel(reelId, wordArray, duration) {
            return new Promise((resolve) => {
                const reel = document.getElementById(reelId);
                const startTime = Date.now();
                let position = 0;
                
                const animate = () => {
                    const elapsed = Date.now() - startTime;
                    
                    if (elapsed < duration) {
                        position += 20;
                        if (position >= 80) {
                            position = 0;
                            reel.textContent = getRandomWord(wordArray);
                        }
                        reel.style.transform = `translateY(${position}px)`;
                        requestAnimationFrame(animate);
                    } else {
                        reel.style.transform = 'translateY(0)';
                        reel.textContent = getRandomWord(wordArray);
                        resolve();
                    }
                };
                
                animate();
            });
        }
        
        async function spin() {
            if (isSpinning || remainingTurns <= 0) return;
            
            isSpinning = true;
            remainingTurns--;
                      document.getElementById('remainingTurns').textContent = remainingTurns;
            document.getElementById('spinButton').disabled = true;
            document.getElementById('result').innerHTML = '<div style="color: #feca57;">スピン中...</div>';
            
            try {
                await animateReel('reel1', reel1Words, 1000);
                await new Promise(resolve => setTimeout(resolve, 200));
                
                await animateReel('reel2', reel2Words, 1000);
                await new Promise(resolve => setTimeout(resolve, 200));
                
                await animateReel('reel3', reel3Words, 1000);
                
                const word1 = document.getElementById('reel1').textContent;
                const word2 = document.getElementById('reel2').textContent;
                const word3 = document.getElementById('reel3').textContent;
                
                const points = checkWin(word1, word2, word3);
                
                updateSentence();
                
                if (points === -1) {
                    // 大当たり表示
                    document.getElementById('result').innerHTML = '<div style="color: #feca57;">🎉 大当たり！「空気がない」 🎉</div>';
                    return;
                } else {
                    let resultText = getEncouragementMessage(word1, word2, word3);
                    
                    if (remainingTurns <= 0) {
                        resultText += ' 回数終了！やり直してもっと回数を稼ごう！';
                    }
                    
                    document.getElementById('result').innerHTML = '<div style="color: #feca57;">' + resultText + '</div>';
                }
                
            } catch (error) {
                document.getElementById('result').innerHTML = '<div style="color: #ff6b6b;">もう一度試してみて！</div>';
            } finally {
                isSpinning = false;
                // エンディング画面が表示されていない場合のみスピンボタンを有効化
                if (remainingTurns > 0 && !document.getElementById('endingScreen').classList.contains('show')) {
                    document.getElementById('spinButton').disabled = false;
                }
            }
        }
        
        // 初期設定
        setupTouchControls();
        updateSentence();
    </script>
</body>
</html>
