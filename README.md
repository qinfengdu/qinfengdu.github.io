<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>家庭阅读冒险岛</title>
    <style>
        :root {
            --primary: #4a6fa5;
            --secondary: #ff9e4f;
            --light: #f0f7ff;
            --success: #6bbf59;
        }
                body {
            font-family: 'Comic Sans MS', 'Microsoft YaHei', sans-serif;
            background-color: var(--light);
            margin: 0;
            padding: 20px;
            color: #333;
        }
        header {
            background-color: var(--primary);
            color: white;
            padding: 10px 20px;
            border-radius: 15px;
            text-align: center;
            margin-bottom: 20px;
        }
        h1 {
            margin: 10px 0;
                        font-size: 24px;
        }
        .family-container {
            display: flex;
            flex-wrap: nowrap;
            gap: 20px;
            justify-content: center;
            margin-bottom: 20px;
            overflow-x: auto;
        }
        .member-card {
            background: white;
            border-radius: 15px;
            padding: 15px;
            min-width: 150px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
            text-align: center;
        }
        .member-avatar {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background-color: var(--secondary);
            margin: 0 auto 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 30px; /* 增大Emoji大小 */
        }
                .progress-container {
            margin: 10px 0;
        }
        .progress-bar {
            height: 15px;
            background-color: #e0e0e0;
            border-radius: 10px;
            overflow: hidden;
        }
        .progress-fill {
            height: 100%;
            background-color: var(--success);
            width: 0%;
            transition: width 0.5s ease;
        }
        .checkin-form {
            background: white;
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
            position: relative;
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
                    }
        input, select, textarea {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 8px;
            box-sizing: border-box;
        }
        .checkin-options {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
        }
        .checkin-option {
            flex: 1;
            text-align: center;
            padding: 10px;
            background: #f5f5f5;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s;
        }
        .checkin-option.selected {
            background: var(--secondary);
            color: white;
        }
        button {
            background-color: var(--primary);
            color: white;
            border: none;
                        padding: 12px 20px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s;
            width: 100%;
        }
        button:hover {
            background-color: #3a5a8f;
        }
        .reset-btn {
            position: absolute;
            top: 20px;
            right: 20px;
            background-color: #f44336;
            width: auto;
            padding: 8px 12px;
            font-size: 14px;
        }
        .reset-btn:hover {
                  background-color: #d32f2f;
        }
        .family-goal {
            background: white;
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
            text-align: center;
        }
        .tree-container {
            margin: 20px 0;
        }
        .tree {
            font-size: 80px;
            display: inline-block;
            transition: all 0.5s;
            transform-origin: bottom;
        }
        .tree-small { transform: scale(0.6); opacity: 0.8; }
        .tree-medium { transform: scale(1); }
        .tree-large { transform: scale(1.3); }
        .hidden {
            display: none;
        }
        .member-points {
            font-size: 24px;
            font-weight: bold;
            color: var(--primary);
            margin: 5px 0;
        }
            </style>
</head>
<body>
    <header>
        <h1>家庭阅读冒险岛</h1>
        <p>一起阅读，一起成长！</p>
    </header>

    <div class="family-container" id="family-members">
        <!-- 家庭成员卡片会动态添加 -->
    </div>

    <div class="checkin-form">
        <h2>今日打卡</h2>
        <button class="reset-btn" onclick="resetToday()">重置当日记录</button>
        
        <div class="form-group">
            <label for="member-select">选择家庭成员</label>
            <select id="member-select">
                <option value="">-- 请选择 --</option>
                <!-- 成员选项会动态添加 -->
            </select>
        </div>
        
        <div class="checkin-options">
            <div class="checkin-option selected" data-type="reading" onclick="selectCheckinType(this, 'reading')">📖 阅读打卡</div>
            <div class="checkin-option" data-type="book" onclick="selectCheckinType(this, 'book')">📚 读完一本书</div>
        </div>
        
        <div id="reading-form" class="checkin-type-form">
                    <div class="form-group">
                <label for="book-title">书名</label>
                <input type="text" id="book-title" placeholder="请输入书名">
            </div>
            <div class="form-group">
                <label for="reading-time">阅读时长 (分钟)</label>
                <input type="number" id="reading-time" placeholder="请输入分钟数">
            </div>
        </div>
        
        <div id="book-form" class="checkin-type-form hidden">
            <div class="form-group">
                <label for="finished-book">书名</label>
                <input type="text" id="finished-book" placeholder="请输入书名">
            </div>
            <div class="form-group">
                <label for="book-pages">总页数</label>
                <input type="number" id="book-pages" placeholder="请输入总页数">
            </div>
        </div>
        
        <button onclick="submitCheckin()">提交打卡</button>
    </div>

    <div class="family-goal">
        <h2>家庭目标</h2>
        <p>本周全家阅读总时长目标: <strong>7小时</strong></p>
        <p>已完成: <span id="family-time">0</span>小时</p>
        <div class="progress-container">
            <div class="progress-bar">
                            <div class="progress-fill" id="family-progress"></div>
            </div>
        </div>
        
        <div class="tree-container">
            <div class="tree tree-small" id="family-tree">🌱</div>
        </div>
    </div>

    <script>
        // 家庭成员数据
        let familyMembers = [
            { id: 1, name: "爸爸", emoji: "👨", color: "#4a6fa5", points: 0, time: 0, books: 0 },
            { id: 2, name: "妈妈", emoji: "👩", color: "#c76fd6", points: 0, time: 0, books: 0 },
            { id: 3, name: "孩子", emoji: "🧒", color: "#6bbf59", points: 0, time: 0, books: 0 }
        ];
        
        // 当日打卡记录(用于重置功能)
        let todayCheckins = [];
        
        // 家庭总阅读时间(分钟)
        let familyTotalTime = 0;
        
        // 当前选中的打卡类型
        let currentCheckinType = 'reading';
        
        // 初始化页面
        function initPage() {
            renderFamilyMembers();
            renderCheckinSelect();
                        updateFamilyProgress();
        }
        
        // 渲染家庭成员卡片
        function renderFamilyMembers() {
            const container = document.getElementById('family-members');
            container.innerHTML = '';
            
            familyMembers.forEach(member => {
                const card = document.createElement('div');
                card.className = 'member-card';
                card.innerHTML = `
                    <div class="member-avatar" style="background-color: ${member.color}">${member.emoji}</div>
                    <h3>${member.name}</h3>
                    <div class="member-points">${member.points}分</div>
                    <p>阅读: ${Math.floor(member.time/60)}h${member.time%60}m</p>
                    <p>书籍: ${member.books}本</p>
                    <div class="progress-container">
                        <div class="progress-bar">
                            <div class="progress-fill" style="width: 0%; background-color: ${member.color}"></div>
                         </div>
                    </div>
                `;
                container.appendChild(card);
            });
        }
        
        // 渲染打卡选择下拉框
        function renderCheckinSelect() {
            const select = document.getElementById('member-select');
            select.innerHTML = '<option value="">-- 请选择 --</option>';
            
            familyMembers.forEach(member => {
                const option = document.createElement('option');
                option.value = member.id;
                option.textContent = `${member.emoji} ${member.name}`;
                select.appendChild(option);
            });
        }
        
        // 选择打卡类型
                function selectCheckinType(element, type) {
            // 更新选中状态
            document.querySelectorAll('.checkin-option').forEach(opt => {
                opt.classList.remove('selected');
            });
            element.classList.add('selected');
            
            // 隐藏所有表单
            document.querySelectorAll('.checkin-type-form').forEach(form => {
                form.classList.add('hidden');
                          });
            
            // 显示选中的表单
            document.getElementById(`${type}-form`).classList.remove('hidden');
            
            currentCheckinType = type;
        }
        
        // 提交打卡
        function submitCheckin() {
            const memberId = parseInt(document.getElementById('member-select').value);
            if (!memberId) {
                alert('请选择家庭成员');
                return;
            }
            
            const member = familyMembers.find(m => m.id === memberId);
            if (!member) return;
            
            let points = 0;
            let time = 0;
            let books = 0;  
                        let description = '';
            
            switch (currentCheckinType) {
                case 'reading':
                    const readingTime = parseInt(document.getElementById('reading-time').value) || 0;
                    const bookTitle = document.getElementById('book-title').value.trim();
                    
                    if (!bookTitle) {
                        alert('请输入书名');
                        return;
                    }
                    
                    if (readingTime <= 0) {
                        alert('请输入有效的阅读时间');
                        return;
                    }
                    
                    points = Math.floor(readingTime / 10);
                    time = readingTime;
                    description = `阅读《${bookTitle}》${readingTime}分钟`;
                    break;
                    
                case 'book':
                    const finishedBook = document.getElementById('finished-book').value.trim();    
                                        const bookPages = parseInt(document.getElementById('book-pages').value) || 0;
                    
                    if (!finishedBook) {
                        alert('请输入书名');
                        return;
                    }
                    
                    if (bookPages <= 0) {
                        alert('请输入有效的页数');
                        return;
                    }
                    
                    points = Math.floor(bookPages / 5);
                    books = 1;
                    description = `读完《${finishedBook}》，${bookPages}页`;
                    break;
                          }
            
            // 保存当日打卡记录(用于重置)
            todayCheckins.push({
                memberId,
                points,
                time,
                books
            });
            
            // 更新成员数据
            member.points += points;
            member.time += time;
            member.books += books;
            
            // 更新家庭总时间
            familyTotalTime += time;
            
            // 更新UI
            renderFamilyMembers();
            updateFamilyProgress();
            
            // 重置表单
            document.getElementById('member-select').value = '';
            document.querySelectorAll('input').forEach(input => {
                if (input.type !== 'button' && input.type !== 'submit') {  
                          input.value = '';
                }
            });
            
            alert(`${member.emoji} ${member.name} 打卡成功！获得 ${points} 积分！`);
        }
        
        // 重置当日记录
        function resetToday() {
            if (todayCheckins.length === 0) {
                alert('今日还没有打卡记录');
                return;
            }
            
            if (!confirm('确定要重置今日所有打卡记录吗？')) {
                return;
            }
            
            // 回滚当日所有打卡记录
            todayCheckins.forEach(checkin => {
                const member = familyMembers.find(m => m.id === checkin.memberId);
                if (member) {
                    member.points -= checkin.points;
                    member.time -= checkin.time;
                    member.books -= checkin.books;      
                              familyTotalTime -= checkin.time;
                }
            });
            
            // 清空当日记录
            todayCheckins = [];
            
            // 更新UI
            renderFamilyMembers();
            updateFamilyProgress();
            
            alert('今日打卡记录已重置');
        }
        
        // 更新家庭进度
        function updateFamilyProgress() {
            const hours = Math.floor(familyTotalTime / 60);    
                        const minutes = familyTotalTime % 60;
            
            document.getElementById('family-time').textContent = `${hours}小时${minutes}分钟`;
            
            const progress = Math.min(100, (familyTotalTime / (7 * 60)) * 100);
            document.getElementById('family-progress').style.width = `${progress}%`;
            
            // 更新家庭阅读树
            const tree = document.getElementById('family-tree');
            if (progress >= 100) {
                tree.className = 'tree tree-large';
                tree.textContent = '🌳';
            } else if (progress >= 50) {
                tree.className = 'tree tree-medium';
                tree.textContent = '🌲';
            } else {  
                  tree.className = 'tree tree-small';
                tree.textContent = '🌱';
            }
        }
        
        // 初始化页面
        window.onload = initPage;
    </script>
</body>
</html>
