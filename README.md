# tasktati
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>タスク管理アプリ</title>
    <style>
        @font-face {
            font-family: 'SoukouMincho';
            src: url('path/to/SoukouMincho.ttf') format('truetype');
            /* Webフォントの場合は以下のような形式も追加できます
               src: url('path/to/SoukouMincho.woff2') format('woff2'),
                    url('path/to/SoukouMincho.woff') format('woff');
            */
        }

        body {
            font-family: 'SoukouMincho', Arial, sans-serif;
            padding: 20px;
            background-image: url('IMG_4809.PNG.png');
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
        }

        .task {
            display: flex;
            justify-content: space-between;
            padding: 10px;
            border-bottom: 1px solid #ccc;
            background: rgba(255, 255, 255, 0.8); /* 背景の透明度を調整 */
        }

        .completed {
            text-decoration: line-through;
            color: grey;
        }

        .stats {
            display: flex;
            justify-content: space-between;
            margin-top: 20px;
            background: rgba(255, 255, 255, 0.8); /* 背景の透明度を調整 */
            padding: 10px;
        }

        .stats div {
            font-size: 18px;
        }

        .present-boxes {
            display: flex;
            gap: 10px;
            margin-top: 20px;
        }

        .present-box {
            width: 50px;
            height: 50px;
            border: 2px solid #000;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 24px;
        }

        .completed-box {
            background-color: gold;
        }
    </style>
</head>
<body>
    <h1>タスク管理アプリ</h1>
    <div class="stats">
        <div class="completed-count">完了したタスク数: <span id="completed-count">0</span></div>
        <div class="player-exp">プレイヤーレベル: <span id="player-exp">0Lv</span></div>
        <div class="coins">コイン: <span id="coin-count">0</span></div>
    </div>
    <div class="present-boxes">
        <div class="present-box" id="box-1">🎁</div>
        <div class="present-box" id="box-2">🎁</div>
    </div>
    <form id="task-form">
        <input type="text" id="task-input" placeholder="新しいタスクを入力してください">
        <button type="submit">追加</button>
    </form>
    <ul id="task-list"></ul>
    <div id="message" style="margin-top: 20px; font-size: 18px;"></div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const taskForm = document.getElementById('task-form');
            const taskInput = document.getElementById('task-input');
            const taskList = document.getElementById('task-list');
            const completedCountElement = document.getElementById('completed-count');
            const playerExpElement = document.getElementById('player-exp');
            const coinCountElement = document.getElementById('coin-count');
            const messageElement = document.getElementById('message');
            const presentBoxes = document.querySelectorAll('.present-box');
            let completedCount = 0;
            let playerExp = 0;
            let coins = 0;
            let completedBoxes = 0;

            taskForm.addEventListener('submit', addTask);
            taskList.addEventListener('click', handleTaskClick);

            function addTask(event) {
                event.preventDefault();
                const taskText = taskInput.value.trim();
                if (taskText !== '') {
                    const li = document.createElement('li');
                    li.className = 'task';
                    li.innerHTML = `
                        <span>${taskText}</span>
                        <button class="delete-btn">削除</button>
                        <button class="complete-btn">完了</button>
                    `;
                    taskList.appendChild(li);
                    taskInput.value = '';
                }
            }

            function handleTaskClick(event) {
                if (event.target.classList.contains('delete-btn')) {
                    const task = event.target.parentElement;
                    if (task.classList.contains('completed')) {
                        updateCompletedCount(-1);
                    }
                    taskList.removeChild(task);
                } else if (event.target.classList.contains('complete-btn')) {
                    const task = event.target.parentElement;
                    task.classList.toggle('completed');
                    if (task.classList.contains('completed')) {
                        updateCompletedCount(1);
                    } else {
                        updateCompletedCount(-1);
                    }
                }
            }

            function updateCompletedCount(change) {
                completedCount += change;
                completedCountElement.textContent = completedCount;

                // プレイヤーレベルを更新
                if (completedCount > 10) {
                    PlayerExp(change);
                }

                // 3の倍数のタスク完了ごとにプレゼントボックスを完了
                if (change > 0 && completedCount % 3 === 0) {
                    completePresentBox();
                }
            }

            function PlayerExp(exp) {
                playerExp += exp;
                playerExpElement.textContent = playerExp + 'Lv';
            }

            function completePresentBox() {
                if (completedBoxes < presentBoxes.length) {
                    presentBoxes[completedBoxes].classList.add('completed-box');
                    coins += 100;
                    coinCountElement.textContent = coins;
                    completedBoxes++;
                }
                if (completedBoxes === presentBoxes.length) {
                    messag
