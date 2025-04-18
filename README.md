<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quản Lý Tiến Độ Dự Án - Dark Mode</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
    <link href="https://cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free@6.4.2/css/all.min.css" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/xlsx@0.18.5/dist/xlsx.full.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/html2pdf.js@0.9.3/dist/html2pdf.bundle.min.js"></script>
    <style>
        :root {
            --primary-bg: #011627;
            --secondary-bg: #01111f;
            --accent-bg: #0d2538;
            --text-color: #d6e5f3;
            --primary-accent: #2196F3;
            --secondary-accent: #4CAF50;
            --warning-color: #FFC107;
            --danger-color: #F44336;
            --grid-color: #0a2540;
            --border-color: #0f3158;
        }
        
        body {
            background-color: var(--primary-bg);
            color: var(--text-color);
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        
        .card {
            background-color: var(--secondary-bg);
            border-radius: 8px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
            border: 1px solid var(--border-color);
        }
        
        h1, h2, h3, h4, h5, h6 {
            color: var(--primary-accent);
            margin-bottom: 15px;
        }
        
        .tabs {
            display: flex;
            border-bottom: 1px solid var(--border-color);
            margin-bottom: 20px;
        }
        
        .tab {
            padding: 10px 20px;
            cursor: pointer;
            transition: background-color 0.3s;
            border-radius: 5px 5px 0 0;
            margin-right: 5px;
        }
        
        .tab:hover {
            background-color: var(--accent-bg);
        }
        
        .tab.active {
            background-color: var(--accent-bg);
            border-bottom: 3px solid var(--primary-accent);
            font-weight: bold;
        }
        
        .tab-content {
            display: none;
        }
        
        .tab-content.active {
            display: block;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
            background-color: var(--secondary-bg);
        }
        
        th {
            background-color: var(--accent-bg);
            padding: 12px 15px;
            text-align: left;
            font-weight: bold;
            color: var(--primary-accent);
            border-bottom: 1px solid var(--border-color);
        }
        
        td {
            padding: 10px 15px;
            border-bottom: 1px solid var(--border-color);
        }
        
        tr:hover {
            background-color: var(--accent-bg);
        }
        
        button {
            background-color: var(--primary-accent);
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 4px;
            cursor: pointer;
            transition: background-color 0.3s;
            font-weight: bold;
            margin-right: 5px;
            margin-bottom: 5px;
        }
        
        button:hover {
            opacity: 0.9;
        }
        
        button:disabled {
            background-color: #555;
            cursor: not-allowed;
            opacity: 0.6;
        }
        
        .btn-success {
            background-color: var(--secondary-accent);
        }
        
        .btn-warning {
            background-color: var(--warning-color);
            color: #333;
        }
        
        .btn-danger {
            background-color: var(--danger-color);
        }
        
        .btn-reset {
            background-color: #FF5722;
        }
        
        .btn-login {
            background-color: #9C27B0;
        }
        
        .btn-logout {
            background-color: #607D8B;
        }
        
        input, select, textarea {
            background-color: var(--accent-bg);
            border: 1px solid var(--border-color);
            padding: 8px 10px;
            border-radius: 4px;
            color: var(--text-color);
            width: 100%;
            margin-bottom: 10px;
        }
        
        .progress-bar {
            height: 20px;
            background-color: var(--accent-bg);
            border-radius: 10px;
            overflow: hidden;
            position: relative;
            margin-bottom: 10px;
        }
        
        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, var(--primary-accent), var(--secondary-accent));
            border-radius: 10px;
            transition: width 0.3s;
        }
        
        .progress-text {
            position: absolute;
            right: 10px;
            top: 0;
            line-height: 20px;
            font-weight: bold;
            color: white;
            mix-blend-mode: difference;
        }
        
        .info-table td:first-child {
            font-weight: bold;
            width: 30%;
        }
        
        .gantt-container {
            position: relative;
            margin-top: 20px;
            margin-bottom: 30px;
            padding-bottom: 20px;
            display: flex;
            flex-direction: row;
            height: auto;
        }
        
        .gantt-task-list {
            width: 250px;
            background-color: var(--secondary-bg);
            border: 1px solid var(--border-color);
            border-right: none;
            overflow-y: auto;
            max-height: 500px;
        }
        
        .gantt-task-item {
            padding: 8px 12px;
            border-bottom: 1px solid var(--border-color);
            font-size: 0.9rem;
            cursor: pointer;
        }
        
        .gantt-task-item:hover {
            background-color: var(--accent-bg);
        }
        
        .gantt-task-item.parent {
            font-weight: bold;
            background-color: rgba(33, 150, 243, 0.1);
        }
        
        .gantt-task-item.child {
            padding-left: 25px;
        }
        
        .gantt-task-item.completed {
            color: var(--secondary-accent);
        }
        
        .gantt-task-item.in-progress {
            color: var(--primary-accent);
        }
        
        .gantt-task-item.not-started {
            color: #757575;
        }
        
        .gantt-chart-wrapper {
            flex: 1;
            overflow-x: auto;
            position: relative;
            max-height: 500px;
        }
        
        .gantt-grid {
            position: relative;
            min-height: 300px;
            border: 1px solid var(--border-color);
            background: var(--secondary-bg);
        }
        
        .gantt-grid-lines {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            z-index: 1;
        }
        
        .gantt-grid-line {
            position: absolute;
            top: 0;
            bottom: 0;
            border-left: 1px dashed var(--grid-color);
            z-index: 1;
        }
        
        .gantt-time-labels {
            display: flex;
            font-size: 0.8rem;
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 5px;
            position: relative;
            z-index: 2;
        }
        
        .gantt-time-label {
            position: absolute;
            transform: translateX(-50%);
            font-weight: bold;
        }
        
        .gantt-bar {
            position: absolute;
            height: 25px;
            background: linear-gradient(90deg, var(--primary-accent), var(--secondary-accent));
            border-radius: 4px;
            z-index: 3;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
            transition: all 0.3s;
            cursor: pointer;
        }
        
        .gantt-bar.completed {
            background: linear-gradient(90deg, #388E3C, #7CB342);
        }
        
        .gantt-bar.in-progress {
            background: linear-gradient(90deg, #1976D2, #64B5F6);
        }
        
        .gantt-bar.not-started {
            background: linear-gradient(90deg, #757575, #BDBDBD);
        }
        
        .gantt-bar:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
        }
        
        .gantt-bar-label {
            position: absolute;
            left: 10px;
            top: 4px;
            color: white;
            font-size: 0.8rem;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            max-width: calc(100% - 20px);
        }
        
        .gantt-zoom-controls {
            position: absolute;
            top: 10px;
            right: 10px;
            z-index: 10;
            display: flex;
            background: var(--accent-bg);
            border-radius: 4px;
            padding: 2px;
            border: 1px solid var(--border-color);
        }
        
        .gantt-zoom-controls button {
            width: 30px;
            height: 30px;
            margin: 0;
            padding: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            background: transparent;
            color: var(--text-color);
            border-radius: 2px;
        }
        
        .gantt-zoom-controls button:hover {
            background: var(--primary-accent);
        }
        
        .gantt-tooltip {
            position: absolute;
            background-color: var(--accent-bg);
            border: 1px solid var(--border-color);
            border-radius: 4px;
            padding: 10px;
            z-index: 100;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
            pointer-events: none;
            min-width: 200px;
            max-width: 300px;
            font-size: 0.9rem;
            opacity: 0;
            transition: opacity 0.2s;
        }
        
        .gantt-tooltip-title {
            font-weight: bold;
            margin-bottom: 5px;
            color: var(--primary-accent);
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 5px;
        }
        
        .gantt-tooltip-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 3px;
        }
        
        .gantt-tooltip-label {
            font-weight: bold;
            margin-right: 10px;
        }
        
        .gantt-dependencies {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            pointer-events: none;
            z-index: 2;
        }
        
        .dependency-line {
            position: absolute;
            stroke: #64B5F6;
            stroke-width: 1.5px;
            fill: none;
            pointer-events: none;
        }
        
        .dependency-arrow {
            fill: #64B5F6;
        }
        
        .status-completed {
            color: var(--secondary-accent);
        }
        
        .status-in-progress {
            color: var(--primary-accent);
        }
        
        .status-not-started {
            color: #757575;
        }
        
        .modal {
            display: none;
            position: fixed;
            z-index: 100;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0,0,0,0.7);
        }
        
        .modal-content {
            background-color: var(--secondary-bg);
            margin: 10% auto;
            padding: 20px;
            border: 1px solid var(--border-color);
            border-radius: 8px;
            width: 80%;
            max-width: 600px;
            max-height: 80vh;
            overflow-y: auto;
        }
        
        .close {
            color: #aaa;
            float: right;
            font-size: 28px;
            font-weight: bold;
            cursor: pointer;
        }
        
        .close:hover {
            color: white;
        }
        
        .form-group {
            margin-bottom: 15px;
        }
        
        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        
        .parent-task {
            font-weight: bold;
            background-color: rgba(33, 150, 243, 0.1);
        }
        
        .child-task {
            padding-left: 30px;
        }
        
        .filter-controls {
            margin-bottom: 15px;
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
        }
        
        .header-buttons {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }
        
        .task-completed {
            background-color: rgba(76, 175, 80, 0.1);
        }
        
        .task-in-progress {
            background-color: rgba(33, 150, 243, 0.1);
        }
        
        .payment-completed {
            background-color: rgba(76, 175, 80, 0.1);
        }
        
        .payment-processing {
            background-color: rgba(255, 193, 7, 0.1);
        }
        
        .export-button {
            display: flex;
            align-items: center;
            gap: 5px;
            font-weight: bold;
        }
        
        .export-button i {
            font-size: 1.1rem;
        }
        
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px 20px;
            border-radius: 4px;
            color: white;
            font-weight: bold;
            z-index: 1000;
            opacity: 0;
            transform: translateY(-20px);
            transition: opacity 0.3s, transform 0.3s;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        
        .notification.success {
            background-color: var(--secondary-accent);
        }
        
        .notification.info {
            background-color: var(--primary-accent);
        }
        
        .notification.error {
            background-color: var(--danger-color);
        }
        
        .notification.show {
            opacity: 1;
            transform: translateY(0);
        }
        
        .save-button {
            background-color: #ff9800;
            color: white;
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        .save-button i {
            font-size: 1.1rem;
        }
        
        .autosave-indicator {
            display: inline-flex;
            align-items: center;
            margin-left: 10px;
            font-size: 0.8rem;
            color: #aaa;
            opacity: 0;
            transition: opacity 0.3s;
        }
        
        .autosave-indicator.active {
            opacity: 1;
        }
        
        .last-saved {
            font-size: 0.8rem;
            color: #aaa;
            margin-top: 5px;
            text-align: right;
        }
        
        .login-info {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-left: 20px;
        }
        
        .user-avatar {
            width: 30px;
            height: 30px;
            background-color: var(--primary-accent);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: bold;
        }
        
        .username {
            font-weight: bold;
            color: var(--primary-accent);
        }
        
        .button-group {
            display: flex;
            gap: 10px;
            align-items: center;
        }
        
        /* Styles for PDF export */
         {
            body {
                background-color: white;
                color: black;
            }
            
            .card {
                background-color: white;
                box-shadow: none;
                border: 1px solid #ddd;
            }
            
            .tabs, button, .gantt-zoom-controls {
                display: none !important;
            }
            
            .tab-content {
                display: block !important;
            }
            
            table {
                background-color: white;
            }
            
            th {
                background-color: #f5f5f5;
                color: black;
            }
            
            .progress-fill {
                print-color-adjust: exact;
                -webkit-print-color-adjust: exact;
            }
            
            .gantt-container {
                overflow: visible;
                height: auto;
                page-break-inside: avoid;
            }
            
            .gantt-chart-wrapper {
                overflow: visible;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1 class="text-2xl font-bold mb-6">Quản Lý Tiến Độ Dự Án</h1>
        
        <div class="header-buttons">
            <div class="button-group">
                <button id="saveDataBtn" class="save-button">
                    <i class="fas fa-save"></i> Lưu Dữ Liệu
                </button>
                <span id="autosaveIndicator" class="autosave-indicator">
                    <i class="fas fa-spinner fa-spin mr-1"></i> Đang lưu...
                </span>
                <button id="resetDataBtn" class="btn-reset">
                    <i class="fas fa-trash-restore"></i> Reset Dữ Liệu
                </button>
            </div>
            
            <div class="button-group">
                <button id="exportExcel" class="export-button btn-success">
                    <i class="fas fa-file-excel"></i> Xuất Excel
                </button>
                <button id="exportPDF" class="export-button">
                    <i class="fas fa-file-pdf"></i> Xuất PDF
                </button>
                
                <!-- Login button or user info -->
                <div id="loginContainer">
                    <button id="loginBtn" class="btn-login">
                        <i class="fas fa-sign-in-alt"></i> Đăng Nhập
                    </button>
                </div>
                
                <div id="userInfoContainer" style="display: none;">
                    <div class="login-info">
                        <div class="user-avatar">
                            <i class="fas fa-user"></i>
                        </div>
                        <span id="usernameDisplay" class="username">admin</span>
                        <button id="logoutBtn" class="btn-logout">
                            <i class="fas fa-sign-out-alt"></i> Đăng Xuất
                        </button>
                    </div>
                </div>
            </div>
        </div>
        <div id="lastSaved" class="last-saved"></div>
        
        <div class="tabs">
            <div class="tab active" data-tab="overview">Tổng Quan</div>
            <div class="tab" data-tab="gantt">Biểu Đồ Gantt</div>
            <div class="tab" data-tab="tasks">Chi Tiết Công Việc</div>
            <div class="tab" data-tab="payments">Đợt Thanh Toán</div>
        </div>
        
        <div id="overview" class="tab-content active">
            <div class="card">
                <div class="flex justify-between items-center mb-4">
                    <h2 class="text-xl">Thông Tin Dự Án</h2>
                    <button id="editProjectBtn" class="edit-button">Chỉnh sửa</button>
                </div>
                <table class="info-table">
                    <tr>
                        <td>Tên dự án:</td>
                        <td id="project-name">Khu chung cư Green Paradise</td>
                    </tr>
                    <tr>
                        <td>Chủ đầu tư:</td>
                        <td id="project-investor">Công ty BĐS Xanh Việt</td>
                    </tr>
                    <tr>
                        <td>Đơn vị thi công:</td>
                        <td id="project-contractor">Xây dựng Sông Đà</td>
                    </tr>
                    <tr>
                        <td>Địa điểm:</td>
                        <td id="project-location">Quận 9, TP. Hồ Chí Minh</td>
                    </tr>
                    <tr>
                        <td>Ngày khởi công:</td>
                        <td id="project-start-date">15/01/2023</td>
                    </tr>
                    <tr>
                        <td>Dự kiến hoàn thành:</td>
                        <td id="project-end-date">30/07/2024</td>
                    </tr>
                    <tr>
                        <td>Tổng thời gian:</td>
                        <td id="project-duration">18 tháng 15 ngày</td>
                    </tr>
                </table>
            </div>
            
            <div class="card">
                <h2 class="text-xl mb-4">Thông Tin Tài Chính</h2>
                <table class="info-table">
                    <tr>
                        <td>Tổng giá trị dự án:</td>
                        <td id="project-total-value">16.500.000.000 VNĐ</td>
                    </tr>
                    <tr>
                        <td>Đã thanh toán:</td>
                        <td id="project-paid-value">0 VNĐ (0%)</td>
                    </tr>
                    <tr>
                        <td>Còn lại:</td>
                        <td id="project-remaining-value">16.500.000.000 VNĐ (100%)</td>
                    </tr>
                </table>
                <div class="mt-4">
                    <div class="text-sm mb-1">Tiến độ thanh toán:</div>
                    <div class="progress-bar">
                        <div class="progress-fill" id="payment-progress-bar" style="width: 0%"></div>
                        <div class="progress-text" id="payment-progress-text">0%</div>
                    </div>
                </div>
            </div>
            
            <div class="card">
                <h2 class="text-xl mb-4">Tiến Độ Theo Giai Đoạn</h2>
                <table id="phases-table">
                    <thead>
                        <tr>
                            <th>Giai đoạn</th>
                            <th>Thời gian</th>
                            <th>Tiến độ</th>
                            <th>Trạng thái</th>
                        </tr>
                    </thead>
                    <tbody>
                        <!-- Dữ liệu giai đoạn sẽ được thêm vào đây bằng JavaScript -->
                    </tbody>
                </table>
            </div>
            
            <div class="card">
                <div class="flex justify-between items-center mb-4">
                    <h2 class="text-xl">Đợt Thanh Toán</h2>
                    <button id="addPaymentBtn" class="edit-button">Thêm đợt thanh toán</button>
                </div>
                <table id="payments-table">
                    <thead>
                        <tr>
                            <th>STT</th>
                            <th>Giai đoạn</th>
                            <th>Giá trị (%)</th>
                            <th>Giá trị (VNĐ)</th>
                            <th>Thời gian</th>
                            <th>Trạng thái</th>
                            <th>Thao tác</th>
                        </tr>
                    </thead>
                    <tbody>
                        <!-- Dữ liệu đợt thanh toán sẽ được thêm vào đây bằng JavaScript -->
                    </tbody>
                </table>
            </div>
        </div>
        
        <div id="gantt" class="tab-content">
            <div class="card">
                <h2 class="text-xl mb-4">Biểu Đồ Gantt Tiến Độ Thi Công</h2>
                <div class="filter-controls">
                    <label class="inline-flex items-center">
                        <input type="checkbox" class="form-checkbox" id="show-critical-path" checked>
                        <span class="ml-2">Hiển thị đường găng</span>
                    </label>
                    <label class="inline-flex items-center ml-4">
                        <input type="checkbox" class="form-checkbox" id="show-dependencies">
                        <span class="ml-2">Hiển thị phụ thuộc</span>
                    </label>
                    <label class="inline-flex items-center ml-4">
                        <input type="checkbox" class="form-checkbox" id="show-payments" checked>
                        <span class="ml-2">Hiển thị thanh toán</span>
                    </label>
                </div>
                
                <div class="gantt-container">
                    <!-- Task List Panel -->
                    <div class="gantt-task-list" id="gantt-task-list">
                        <!-- Task items will be added here by JavaScript -->
                    </div>
                    
                    <!-- Gantt Chart Panel -->
                    <div class="gantt-chart-wrapper">
                        <!-- Zoom Controls -->
                        <div class="gantt-zoom-controls">
                            <button id="zoom-in" title="Phóng to"><i class="fas fa-search-plus"></i></button>
                            <button id="zoom-out" title="Thu nhỏ"><i class="fas fa-search-minus"></i></button>
                            <button id="zoom-reset" title="Đặt lại"><i class="fas fa-undo"></i></button>
                        </div>
                        
                        <!-- Tooltip -->
                        <div class="gantt-tooltip" id="gantt-tooltip" style="display:none">
                            <div class="gantt-tooltip-title">Tên công việc</div>
                            <div class="gantt-tooltip-content">
                                <div class="gantt-tooltip-item">
                                    <span class="gantt-tooltip-label">Bắt đầu:</span>
                                    <span class="gantt-tooltip-value" id="tooltip-start-date"></span>
                                </div>
                                <div class="gantt-tooltip-item">
                                    <span class="gantt-tooltip-label">Kết thúc:</span>
                                    <span class="gantt-tooltip-value" id="tooltip-end-date"></span>
                                </div>
                                <div class="gantt-tooltip-item">
                                    <span class="gantt-tooltip-label">Thời lượng:</span>
                                    <span class="gantt-tooltip-value" id="tooltip-duration"></span>
                                </div>
                                <div class="gantt-tooltip-item">
                                    <span class="gantt-tooltip-label">Tiến độ:</span>
                                    <span class="gantt-tooltip-value" id="tooltip-progress"></span>
                                </div>
                                <div class="gantt-tooltip-item">
                                    <span class="gantt-tooltip-label">Trạng thái:</span>
                                    <span class="gantt-tooltip-value" id="tooltip-status"></span>
                                </div>
                            </div>
                        </div>
                        
                        <!-- Gantt Chart -->
                        <div id="gantt-chart" class="gantt-grid">
                            <div class="gantt-time-labels"></div>
                            <div class="gantt-grid-lines"></div>
                            <div class="gantt-dependencies"></div>
                            <div class="gantt-bars"></div>
                        </div>
                    </div>
                </div>
                
                <div>
                    <h3 class="text-lg mb-2">Đường Găng (Critical Path)</h3>
                    <p class="text-sm mb-3">Chuỗi công việc quan trọng ảnh hưởng đến thời gian hoàn thành dự án. Đường găng là chuỗi các công việc mà nếu chậm trễ sẽ làm chậm toàn bộ dự án. Cần ưu tiên đảm bảo tiến độ cho các công việc này.</p>
                    <table id="critical-path-table">
                        <thead>
                            <tr>
                                <th>ID</th>
                                <th>Tên công việc</th>
                                <th>Ngày bắt đầu</th>
                                <th>Ngày kết thúc</th>
                                <th>Tiến độ</th>
                            </tr>
                        </thead>
                        <tbody>
                            <!-- Dữ liệu đường găng sẽ được thêm vào đây bằng JavaScript -->
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
        
        <div id="tasks" class="tab-content">
            <div class="card">
                <div class="flex justify-between items-center mb-4">
                    <h2 class="text-xl">Chi Tiết Tiến Độ Công Việc</h2>
                    <button id="addTaskBtn" class="edit-button">Thêm công việc</button>
                </div>
                <table id="tasks-table">
                    <thead>
                        <tr>
                            <th>ID</th>
                            <th>Công việc</th>
                            <th>Ngày bắt đầu</th>
                            <th>Ngày kết thúc</th>
                            <th>Thời lượng</th>
                            <th>Tiến độ</th>
                            <th>Trạng thái</th>
                            <th>Người phụ trách</th>
                            <th>Thao tác</th>
                        </tr>
                    </thead>
                    <tbody>
                        <!-- Dữ liệu công việc sẽ được thêm vào đây bằng JavaScript -->
                    </tbody>
                </table>
            </div>
        </div>
        
        <div id="payments" class="tab-content">
            <div class="card">
                <div class="flex justify-between items-center mb-4">
                    <h2 class="text-xl">Đợt Thanh Toán</h2>
                    <button id="addPaymentBtn2" class="edit-button">Thêm đợt thanh toán</button>
                </div>
                <table id="payments-table2">
                    <thead>
                        <tr>
                            <th>STT</th>
                            <th>Giai đoạn</th>
                            <th>Giá trị (%)</th>
                            <th>Giá trị (VNĐ)</th>
                            <th>Thời gian</th>
                            <th>Trạng thái</th>
                            <th>Thao tác</th>
                        </tr>
                    </thead>
                    <tbody>
                        <!-- Dữ liệu đợt thanh toán sẽ được thêm vào đây bằng JavaScript -->
                    </tbody>
                </table>
            </div>
        </div>
        
        <!-- Modal chỉnh sửa thông tin dự án -->
        <div id="editProjectModal" class="modal">
            <div class="modal-content">
                <span class="close" id="closeEditProjectModal">&times;</span>
                <h2 class="text-xl mb-4">Chỉnh sửa thông tin dự án</h2>
                <form id="editProjectForm">
                    <div class="form-group">
                        <label for="edit-project-name">Tên dự án:</label>
                        <input type="text" id="edit-project-name" required>
                    </div>
                    <div class="form-group">
                        <label for="edit-project-investor">Chủ đầu tư:</label>
                        <input type="text" id="edit-project-investor" required>
                    </div>
                    <div class="form-group">
                        <label for="edit-project-contractor">Đơn vị thi công:</label>
                        <input type="text" id="edit-project-contractor" required>
                    </div>
                    <div class="form-group">
                        <label for="edit-project-location">Địa điểm:</label>
                        <input type="text" id="edit-project-location" required>
                    </div>
                    <div class="form-group">
                        <label for="edit-project-start-date">Ngày khởi công:</label>
                        <input type="date" id="edit-project-start-date" required>
                    </div>
                    <div class="form-group">
                        <label for="edit-project-end-date">Dự kiến hoàn thành:</label>
                        <input type="date" id="edit-project-end-date" required>
                    </div>
                    <div class="form-group">
                        <label for="edit-project-total-value">Tổng giá trị dự án (VNĐ):</label>
                        <input type="number" id="edit-project-total-value" min="0" step="1000000" required>
                    </div>
                    <button type="submit" class="btn-success">Lưu thay đổi</button>
                </form>
            </div>
        </div>
        
        <!-- Modal thêm/chỉnh sửa công việc -->
        <div id="taskModal" class="modal">
            <div class="modal-content">
                <span class="close" id="closeTaskModal">&times;</span>
                <h2 id="taskModalTitle" class="text-xl mb-4">Thêm công việc mới</h2>
                <form id="taskForm">
                    <input type="hidden" id="task-id">
                    <div class="form-group">
                        <label for="task-name">Tên công việc:</label>
                        <input type="text" id="task-name" required>
                    </div>
                    <div class="form-group">
                        <label for="task-parent">Công việc cha (để trống nếu là công việc cấp 1):</label>
                        <select id="task-parent">
                            <option value="">Không có</option>
                            <!-- Các công việc cha sẽ được thêm vào đây bằng JavaScript -->
                        </select>
                    </div>
                    <div class="form-group">
                        <label for="task-start-date">Ngày bắt đầu:</label>
                        <input type="date" id="task-start-date" required>
                    </div>
                    <div class="form-group">
                        <label for="task-end-date">Ngày kết thúc:</label>
                        <input type="date" id="task-end-date" required>
                    </div>
                    <div class="form-group">
                        <label for="task-progress">Tiến độ (%):</label>
                        <input type="number" id="task-progress" min="0" max="100" value="0" required>
                    </div>
                    <div class="form-group">
                        <label for="task-status">Trạng thái:</label>
                        <select id="task-status" required>
                            <option value="Chưa bắt đầu">Chưa bắt đầu</option>
                            <option value="Đang thực hiện">Đang thực hiện</option>
                            <option value="Hoàn thành">Hoàn thành</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label for="task-assignee">Người phụ trách:</label>
                        <input type="text" id="task-assignee" required>
                    </div>
                    <button type="submit" class="btn-success">Lưu công việc</button>
                </form>
            </div>
        </div>
        
        <!-- Modal thêm/chỉnh sửa đợt thanh toán -->
        <div id="paymentModal" class="modal">
            <div class="modal-content">
                <span class="close" id="closePaymentModal">&times;</span>
                <h2 id="paymentModalTitle" class="text-xl mb-4">Thêm đợt thanh toán mới</h2>
                <form id="paymentForm">
                    <input type="hidden" id="payment-id">
                    <div class="form-group">
                        <label for="payment-phase">Giai đoạn:</label>
                        <input type="text" id="payment-phase" required>
                    </div>
                    <div class="form-group">
                        <label for="payment-percent">Giá trị (%):</label>
                        <input type="number" id="payment-percent" min="0" max="100" step="0.01" required>
                    </div>
                    <div class="form-group">
                        <label for="payment-date">Thời gian:</label>
                        <input type="date" id="payment-date" required>
                    </div>
                    <div class="form-group">
                        <label for="payment-status">Trạng thái:</label>
                        <select id="payment-status" required>
                            <option value="Chưa đến hạn">Chưa đến hạn</option>
                            <option value="Đang xử lý">Đang xử lý</option>
                            <option value="Đã thanh toán">Đã thanh toán</option>
                        </select>
                    </div>
                    <button type="submit" class="btn-success">Lưu đợt thanh toán</button>
                </form>
            </div>
        </div>
        
        <!-- Modal đăng nhập -->
        <div id="loginModal" class="modal">
            <div class="modal-content" style="max-width: 400px;">
                <span class="close" id="closeLoginModal">&times;</span>
                <h2 class="text-xl mb-4 text-center">Đăng Nhập</h2>
                <form id="loginForm">
                    <div class="form-group">
                        <label for="username">Tên đăng nhập:</label>
                        <input type="text" id="username" required>
                    </div>
                    <div class="form-group">
                        <label for="password">Mật khẩu:</label>
                        <input type="password" id="password" required>
                    </div>
                    <div class="text-center">
                        <button type="submit" class="btn-login">
                            <i class="fas fa-sign-in-alt mr-2"></i> Đăng Nhập
                        </button>
                    </div>
                </form>
            </div>
        </div>
        
        <!-- Modal xác nhận reset dữ liệu -->
        <div id="resetConfirmModal" class="modal">
            <div class="modal-content" style="max-width: 400px;">
                <span class="close" id="closeResetConfirmModal">&times;</span>
                <h2 class="text-xl mb-4 text-center">Xác nhận Reset Dữ Liệu</h2>
                <p class="mb-4 text-center">Bạn có chắc chắn muốn xóa tất cả dữ liệu đã lưu và đưa ứng dụng về trạng thái ban đầu?</p>
                <p class="mb-4 text-center text-red-500">Lưu ý: Hành động này không thể hoàn tác!</p>
                <div class="flex justify-center gap-4">
                    <button id="confirmResetBtn" class="btn-danger">
                        <i class="fas fa-trash mr-2"></i> Xác nhận Reset
                    </button>
                    <button id="cancelResetBtn" class="btn-secondary">
                        <i class="fas fa-times mr-2"></i> Hủy bỏ
                    </button>
                </div>
            </div>
        </div>
        
        <!-- Notification -->
        <div id="notification" class="notification"></div>
    </div>
    
    <script>
        // Dữ liệu ứng dụng
        let projectData = {
            name: "Khu chung cư Green Paradise",
            investor: "Công ty BĐS Xanh Việt",
            contractor: "Xây dựng Sông Đà",
            location: "Quận 9, TP. Hồ Chí Minh",
            startDate: "2023-01-15",
            endDate: "2024-07-30",
            totalValue: 16500000000,
            paidValue: 0,
            paidPercent: 0
        };
        
        let tasks = [
            {
                id: 1, 
                name: "Chuẩn bị", 
                startDate: "2023-01-15", 
                endDate: "2023-02-28", 
                progress: 100, 
                status: "Hoàn thành", 
                assignee: "Ban Quản Lý", 
                parentId: null,
                critical: true
            },
            {
                id: 101, 
                name: "Chuẩn bị mặt bằng", 
                startDate: "2023-01-15", 
                endDate: "2023-02-15", 
                progress: 100, 
                status: "Hoàn thành", 
                assignee: "Nguyễn Văn A", 
                parentId: 1,
                critical: true
            },
            {
                id: 102, 
                name: "Thiết kế kỹ thuật", 
                startDate: "2023-01-15", 
                endDate: "2023-02-28", 
                progress: 100, 
                status: "Hoàn thành", 
                assignee: "Trần Thị B", 
                parentId: 1,
                critical: false
            },
            {
                id: 2, 
                name: "Thi công móng", 
                startDate: "2023-02-16", 
                endDate: "2023-04-30", 
                progress: 100, 
                status: "Hoàn thành", 
                assignee: "Ban Thi Công", 
                parentId: null,
                critical: true
            },
            {
                id: 201, 
                name: "Đào móng", 
                startDate: "2023-02-16", 
                endDate: "2023-03-15", 
                progress: 100, 
                status: "Hoàn thành", 
                assignee: "Lê Văn C", 
                parentId: 2,
                critical: true
            },
            {
                id: 202, 
                name: "Đổ móng", 
                startDate: "2023-03-16", 
                endDate: "2023-04-30", 
                progress: 100, 
                status: "Hoàn thành", 
                assignee: "Nguyễn Văn E", 
                parentId: 2,
                critical: true
            },
            {
                id: 3, 
                name: "Thi công tầng hầm", 
                startDate: "2023-05-01", 
                endDate: "2023-06-30", 
                progress: 100, 
                status: "Hoàn thành", 
                assignee: "Lê Thị G", 
                parentId: null,
                critical: true
            },
            {
                id: 4, 
                name: "Thi công khung", 
                startDate: "2023-07-01", 
                endDate: "2024-02-28", 
                progress: 65, 
                status: "Đang thực hiện", 
                assignee: "Ban Thi Công", 
                parentId: null,
                critical: true
            },
            {
                id: 401, 
                name: "Thi công khung tầng 1-10", 
                startDate: "2023-07-01", 
                endDate: "2023-11-15", 
                progress: 100, 
                status: "Hoàn thành", 
                assignee: "Nguyễn Thị I", 
                parentId: 4,
                critical: true
            },
            {
                id: 402, 
                name: "Thi công khung tầng 11-20", 
                startDate: "2023-11-16", 
                endDate: "2024-02-28", 
                progress: 30, 
                status: "Đang thực hiện", 
                assignee: "Trần Văn K", 
                parentId: 4,
                critical: true
            },
            {
                id: 5, 
                name: "Hoàn thiện", 
                startDate: "2024-03-01", 
                endDate: "2024-07-15", 
                progress: 0, 
                status: "Chưa bắt đầu", 
                assignee: "Ban Hoàn Thiện", 
                parentId: null,
                critical: true
            },
            {
                id: 501, 
                name: "Hoàn thiện mặt ngoài", 
                startDate: "2024-03-01", 
                endDate: "2024-04-30", 
                progress: 0, 
                status: "Chưa bắt đầu", 
                assignee: "Phạm Thị M", 
                parentId: 5,
                critical: true
            },
            {
                id: 502, 
                name: "Hoàn thiện nội thất chung", 
                startDate: "2024-05-01", 
                endDate: "2024-07-15", 
                progress: 0, 
                status: "Chưa bắt đầu", 
                assignee: "Nguyễn Thị N", 
                parentId: 5,
                critical: true
            },
            {
                id: 6, 
                name: "Bàn giao", 
                startDate: "2024-07-16", 
                endDate: "2024-07-30", 
                progress: 0, 
                status: "Chưa bắt đầu", 
                assignee: "Ban Quản Lý", 
                parentId: null,
                critical: true
            },
            {
                id: 601, 
                name: "Bàn giao công trình", 
                startDate: "2024-07-16", 
                endDate: "2024-07-30", 
                progress: 0, 
                status: "Chưa bắt đầu", 
                assignee: "Nguyễn Văn O", 
                parentId: 6,
                critical: true
            }
        ];
        
        let payments = [
            {
                id: 1,
                phase: "Tạm ứng hợp đồng",
                percent: 20,
                date: "2023-01-15",
                status: "Đã thanh toán"
            },
            {
                id: 2,
                phase: "Hoàn thành móng",
                percent: 30,
                date: "2023-06-30",
                status: "Đã thanh toán"
            },
            {
                id: 3,
                phase: "Hoàn thành khung tầng 10",
                percent: 20,
                date: "2023-11-15",
                status: "Đang xử lý"
            },
            {
                id: 4,
                phase: "Hoàn thành khung tầng 20",
                percent: 15,
                date: "2024-02-28",
                status: "Chưa đến hạn"
            },
            {
                id: 5,
                phase: "Hoàn thiện",
                percent: 10,
                date: "2024-07-15",
                status: "Chưa đến hạn"
            },
            {
                id: 6,
                phase: "Bàn giao",
                percent: 5,
                date: "2024-07-30",
                status: "Chưa đến hạn"
            }
        ];
        
        let phases = [];
        let zoomLevel = 1; // Normal zoom level
        let isAutoSaving = false;
        let autoSaveTimeout;
        let isAuthenticated = false; // Trạng thái đăng nhập
        
        // Hàm tiện ích
        function formatCurrency(amount) {
            return new Intl.NumberFormat('vi-VN').format(amount) + " VNĐ";
        }
        
        function formatDate(dateString) {
            const date = new Date(dateString);
            return date.toLocaleDateString('vi-VN');
        }
        
        function calculateDuration(startDate, endDate) {
            const start = new Date(startDate);
            const end = new Date(endDate);
            const diffTime = Math.abs(end - start);
            return Math.ceil(diffTime / (1000 * 60 * 60 * 24)) + 1;
        }
        
        // Cập nhật phases từ tasks
        function updatePhases() {
            // Clear current phases
            phases = [];
            
            // Get parent tasks
            const parentTasks = tasks.filter(task => task.parentId === null);
            
            // Convert parent tasks to phases
            parentTasks.forEach(task => {
                // Get all child tasks for this parent
                const childTasks = tasks.filter(t => t.parentId === task.id);
                
                // Calculate average progress
                let totalProgress = task.progress;
                let totalItems = 1; // Count the parent task itself
                
                if (childTasks.length > 0) {
                    childTasks.forEach(child => {
                        totalProgress += child.progress;
                        totalItems++;
                    });
                }
                
                const avgProgress = Math.round(totalProgress / totalItems);
                
                // Determine status based on progress
                let status;
                if (avgProgress === 100) {
                    status = "Hoàn thành";
                } else if (avgProgress > 0) {
                    status = "Đang thực hiện";
                } else {
                    status = "Chưa bắt đầu";
                }
                
                phases.push({
                    name: task.name,
                    startDate: task.startDate,
                    endDate: task.endDate,
                    progress: avgProgress,
                    status: status
                });
            });
        }
        
        function updateFinancialInfo() {
            let paidValue = 0;
            let totalValue = projectData.totalValue;
            
            for (let payment of payments) {
                if (payment.status === "Đã thanh toán") {
                    paidValue += (payment.percent / 100) * totalValue;
                }
            }
            
            projectData.paidValue = paidValue;
            projectData.paidPercent = (paidValue / totalValue) * 100;
            
            document.getElementById('project-total-value').textContent = formatCurrency(totalValue);
            document.getElementById('project-paid-value').textContent = `${formatCurrency(paidValue)} (${projectData.paidPercent.toFixed(0)}%)`;
            document.getElementById('project-remaining-value').textContent = `${formatCurrency(totalValue - paidValue)} (${(100 - projectData.paidPercent).toFixed(0)}%)`;
            
            document.getElementById('payment-progress-bar').style.width = `${projectData.paidPercent}%`;
            document.getElementById('payment-progress-text').textContent = `${projectData.paidPercent.toFixed(0)}%`;
        }
        
        // Hàm authentication
        function initializeAuth() {
            // Kiểm tra nếu chưa có tài khoản admin thì tạo mới
            if (!localStorage.getItem('adminCredentials')) {
                localStorage.setItem('adminCredentials', JSON.stringify({
                    username: 'admin',
                    password: 'admin123'
                }));
            }
            
            // Kiểm tra trạng thái đăng nhập lưu trong localStorage
            const authState = localStorage.getItem('authState');
            if (authState) {
                const parsedAuthState = JSON.parse(authState);
                isAuthenticated = parsedAuthState.isAuthenticated;
                
                if (isAuthenticated) {
                    document.getElementById('loginContainer').style.display = 'none';
                    document.getElementById('userInfoContainer').style.display = 'flex';
                    document.getElementById('usernameDisplay').textContent = parsedAuthState.username;
                }
            }
            
            // Cập nhật UI trên toàn bộ app dựa vào trạng thái đăng nhập
            updateUIBasedOnAuth();
        }
        
        function login(username, password) {
            const adminCredentials = JSON.parse(localStorage.getItem('adminCredentials'));
            
            if (username === adminCredentials.username && password === adminCredentials.password) {
                isAuthenticated = true;
                
                // Lưu trạng thái đăng nhập
                localStorage.setItem('authState', JSON.stringify({
                    isAuthenticated: true,
                    username: username
                }));
                
                // Cập nhật UI
                document.getElementById('loginContainer').style.display = 'none';
                document.getElementById('userInfoContainer').style.display = 'flex';
                document.getElementById('usernameDisplay').textContent = username;
                
                // Đóng modal đăng nhập
                document.getElementById('loginModal').style.display = 'none';
                
                // Thông báo
                showNotification('Đăng nhập thành công!', 'success');
                
                // Cập nhật UI dựa trên trạng thái đăng nhập
                updateUIBasedOnAuth();
                
                return true;
            } else {
                showNotification('Tên đăng nhập hoặc mật khẩu không đúng!', 'error');
                return false;
            }
        }
        
        function logout() {
            isAuthenticated = false;
            
            // Xóa trạng thái đăng nhập
            localStorage.removeItem('authState');
            
            // Cập nhật UI
            document.getElementById('loginContainer').style.display = 'block';
            document.getElementById('userInfoContainer').style.display = 'none';
            
            // Thông báo
            showNotification('Đã đăng xuất thành công!', 'info');
            
            // Cập nhật UI dựa trên trạng thái đăng nhập
            updateUIBasedOnAuth();
        }
        
        function updateUIBasedOnAuth() {
            // Điều chỉnh trạng thái các nút chỉnh sửa
            const editButtons = document.querySelectorAll('.edit-button');
            editButtons.forEach(button => {
                button.disabled = !isAuthenticated;
                if (!isAuthenticated) {
                    button.title = "Vui lòng đăng nhập để chỉnh sửa";
                } else {
                    button.title = "";
                }
            });
            
            // Điều chỉnh trạng thái nút lưu dữ liệu và reset dữ liệu
            document.getElementById('saveDataBtn').disabled = !isAuthenticated;
            document.getElementById('resetDataBtn').disabled = !isAuthenticated;
            
            if (!isAuthenticated) {
                document.getElementById('saveDataBtn').title = "Vui lòng đăng nhập để lưu dữ liệu";
                document.getElementById('resetDataBtn').title = "Vui lòng đăng nhập để reset dữ liệu";
            } else {
                document.getElementById('saveDataBtn').title = "Lưu dữ liệu";
                document.getElementById('resetDataBtn').title = "Reset dữ liệu";
            }
        }
        
        // Reset data function
        function resetData() {
            // Xóa tất cả dữ liệu trong localStorage liên quan đến ứng dụng
            localStorage.removeItem('projectManagementData');
            
            // Giữ lại thông tin đăng nhập
            const authState = localStorage.getItem('authState');
            const adminCredentials = localStorage.getItem('adminCredentials');
            
            // Reload trang để reset dữ liệu về ban đầu
            window.location.reload();
        }
        
        // Render dữ liệu
        function renderTasksTable() {
            const tbody = document.querySelector('#tasks-table tbody');
            tbody.innerHTML = '';
            
            tasks.forEach(task => {
                const row = document.createElement('tr');
                if (task.parentId === null) {
                    row.classList.add('parent-task');
                } else {
                    row.classList.add('child-task');
                }
                
                if (task.status === "Hoàn thành") {
                    row.classList.add('task-completed');
                } else if (task.status === "Đang thực hiện") {
                    row.classList.add('task-in-progress');
                }
                
                row.innerHTML = `
                    <td>${task.id}</td>
                    <td>${task.name}</td>
                    <td>${formatDate(task.startDate)}</td>
                    <td>${formatDate(task.endDate)}</td>
                    <td>${calculateDuration(task.startDate, task.endDate)} ngày</td>
                    <td>
                        <div class="progress-bar">
                            <div class="progress-fill" style="width: ${task.progress}%"></div>
                            <div class="progress-text">${task.progress}%</div>
                        </div>
                    </td>
                    <td class="status-${task.status === 'Hoàn thành' ? 'completed' : task.status === 'Đang thực hiện' ? 'in-progress' : 'not-started'}">${task.status}</td>
                    <td>${task.assignee}</td>
                    <td>
                        <button class="btn-primary edit-task edit-button" data-id="${task.id}" ${!isAuthenticated ? 'disabled' : ''}>Sửa</button>
                        <button class="btn-danger delete-task edit-button" data-id="${task.id}" ${!isAuthenticated ? 'disabled' : ''}>Xóa</button>
                        ${task.parentId === null ? `
                            <button class="btn-warning move-up-task edit-button" data-id="${task.id}" ${!isAuthenticated ? 'disabled' : ''}>↑</button>
                            <button class="btn-warning move-down-task edit-button" data-id="${task.id}" ${!isAuthenticated ? 'disabled' : ''}>↓</button>
                        ` : ''}
                    </td>
                `;
                tbody.appendChild(row);
            });
            
            // Thêm event listeners cho các nút nếu đã đăng nhập
            if (isAuthenticated) {
                document.querySelectorAll('.edit-task').forEach(button => {
                    button.addEventListener('click', function() {
                        const taskId = parseInt(this.getAttribute('data-id'));
                        editTask(taskId);
                    });
                });
                
                document.querySelectorAll('.delete-task').forEach(button => {
                    button.addEventListener('click', function() {
                        const taskId = parseInt(this.getAttribute('data-id'));
                        deleteTask(taskId);
                    });
                });
                
                document.querySelectorAll('.move-up-task').forEach(button => {
                    button.addEventListener('click', function() {
                        const taskId = parseInt(this.getAttribute('data-id'));
                        moveTask(taskId, 'up');
                    });
                });
                
                document.querySelectorAll('.move-down-task').forEach(button => {
                    button.addEventListener('click', function() {
                        const taskId = parseInt(this.getAttribute('data-id'));
                        moveTask(taskId, 'down');
                    });
                });
            }
        }
        
        function renderPaymentsTable() {
            const tables = document.querySelectorAll('#payments-table tbody, #payments-table2 tbody');
            
            tables.forEach(tbody => {
                tbody.innerHTML = '';
                
                payments.forEach((payment, index) => {
                    const row = document.createElement('tr');
                    if (payment.status === "Đã thanh toán") {
                        row.classList.add('payment-completed');
                    } else if (payment.status === "Đang xử lý") {
                        row.classList.add('payment-processing');
                    }
                    
                    row.innerHTML = `
                        <td>${index + 1}</td>
                        <td>${payment.phase}</td>
                        <td>${payment.percent}%</td>
                        <td>${formatCurrency((payment.percent / 100) * projectData.totalValue)}</td>
                        <td>${formatDate(payment.date)}</td>
                        <td>${payment.status}</td>
                        <td>
                            <button class="btn-primary edit-payment edit-button" data-id="${payment.id}" ${!isAuthenticated ? 'disabled' : ''}>Sửa</button>
                            <button class="btn-danger delete-payment edit-button" data-id="${payment.id}" ${!isAuthenticated ? 'disabled' : ''}>Xóa</button>
                        </td>
                    `;
                    tbody.appendChild(row);
                });
            });
            
            // Thêm event listeners cho các nút nếu đã đăng nhập
            if (isAuthenticated) {
                document.querySelectorAll('.edit-payment').forEach(button => {
                    button.addEventListener('click', function() {
                        const paymentId = parseInt(this.getAttribute('data-id'));
                        editPayment(paymentId);
                    });
                });
                
                document.querySelectorAll('.delete-payment').forEach(button => {
                    button.addEventListener('click', function() {
                        const paymentId = parseInt(this.getAttribute('data-id'));
                        deletePayment(paymentId);
                    });
                });
            }
        }
        
        function renderPhasesTable() {
            const tbody = document.querySelector('#phases-table tbody');
            tbody.innerHTML = '';
            
            phases.forEach(phase => {
                const row = document.createElement('tr');
                if (phase.status === "Hoàn thành") {
                    row.classList.add('task-completed');
                } else if (phase.status === "Đang thực hiện") {
                    row.classList.add('task-in-progress');
                }
                
                row.innerHTML = `
                    <td>${phase.name}</td>
                    <td>${formatDate(phase.startDate)} - ${formatDate(phase.endDate)}</td>
                    <td>
                        <div class="progress-bar">
                            <div class="progress-fill" style="width: ${phase.progress}%"></div>
                            <div class="progress-text">${phase.progress}%</div>
                        </div>
                    </td>
                    <td class="status-${phase.status === 'Hoàn thành' ? 'completed' : phase.status === 'Đang thực hiện' ? 'in-progress' : 'not-started'}">${phase.status}</td>
                `;
                tbody.appendChild(row);
            });
        }
        
        function renderCriticalPathTable() {
            const tbody = document.querySelector('#critical-path-table tbody');
            tbody.innerHTML = '';
            
            const criticalTasks = tasks.filter(task => task.critical && task.parentId !== null);
            
            criticalTasks.forEach(task => {
                const row = document.createElement('tr');
                if (task.status === "Hoàn thành") {
                    row.classList.add('task-completed');
                } else if (task.status === "Đang thực hiện") {
                    row.classList.add('task-in-progress');
                }
                
                row.innerHTML = `
                    <td>${task.id}</td>
                    <td>${task.name}</td>
                    <td>${formatDate(task.startDate)}</td>
                    <td>${formatDate(task.endDate)}</td>
                    <td>
                        <div class="progress-bar">
                            <div class="progress-fill" style="width: ${task.progress}%"></div>
                            <div class="progress-text">${task.progress}%</div>
                        </div>
                    </td>
                `;
                tbody.appendChild(row);
            });
        }
        
        function renderTaskList() {
            const taskList = document.getElementById('gantt-task-list');
            taskList.innerHTML = '';
            
            tasks.forEach(task => {
                const taskItem = document.createElement('div');
                taskItem.className = 'gantt-task-item';
                
                if (task.parentId === null) {
                    taskItem.classList.add('parent');
                } else {
                    taskItem.classList.add('child');
                }
                
                if (task.status === "Hoàn thành") {
                    taskItem.classList.add('completed');
                } else if (task.status === "Đang thực hiện") {
                    taskItem.classList.add('in-progress');
                } else {
                    taskItem.classList.add('not-started');
                }
                
                taskItem.textContent = task.name;
                taskItem.setAttribute('data-id', task.id);
                
                // Add event listener to highlight corresponding bar in Gantt chart
                taskItem.addEventListener('mouseover', function() {
                    const taskId = parseInt(this.getAttribute('data-id'));
                    const bar = document.querySelector(`.gantt-bar[data-id="${taskId}"]`);
                    if (bar) {
                        bar.style.transform = 'translateY(-2px)';
                        bar.style.boxShadow = '0 4px 8px rgba(0, 0, 0, 0.3)';
                    }
                });
                
                taskItem.addEventListener('mouseout', function() {
                    const taskId = parseInt(this.getAttribute('data-id'));
                    const bar = document.querySelector(`.gantt-bar[data-id="${taskId}"]`);
                    if (bar) {
                        bar.style.transform = '';
                        bar.style.boxShadow = '0 2px 4px rgba(0, 0, 0, 0.2)';
                    }
                });
                
                // Add click event to scroll to the task in Gantt chart
                taskItem.addEventListener('click', function() {
                    const taskId = parseInt(this.getAttribute('data-id'));
                    const bar = document.querySelector(`.gantt-bar[data-id="${taskId}"]`);
                    if (bar) {
                        const chartWrapper = document.querySelector('.gantt-chart-wrapper');
                        const barLeft = bar.offsetLeft;
                        chartWrapper.scrollLeft = barLeft - 100; // Scroll to position with some margin
                    }
                });
                
                taskList.appendChild(taskItem);
            });
        }
        
        function renderGanttChart() {
            const ganttChart = document.getElementById('gantt-chart');
            const timeLabelsContainer = ganttChart.querySelector('.gantt-time-labels');
            const gridLinesContainer = ganttChart.querySelector('.gantt-grid-lines');
            const dependenciesContainer = ganttChart.querySelector('.gantt-dependencies') || document.createElement('div');
            dependenciesContainer.className = 'gantt-dependencies';
            
            // Xóa các thanh cũ
            const oldBarsContainer = ganttChart.querySelector('.gantt-bars');
            if (oldBarsContainer) {
                ganttChart.removeChild(oldBarsContainer);
            }
            
            // Tạo barsContainer mới
            const barsContainer = document.createElement('div');
            barsContainer.className = 'gantt-bars';
            ganttChart.appendChild(barsContainer);
            
            // Cập nhật dependenciesContainer
            if (!ganttChart.querySelector('.gantt-dependencies')) {
                ganttChart.appendChild(dependenciesContainer);
            } else {
                dependenciesContainer.innerHTML = '';
            }
            
            // Tìm thời gian bắt đầu và kết thúc của toàn bộ dự án
            const projectStartDate = new Date(projectData.startDate);
            const projectEndDate = new Date(projectData.endDate);
            
            // Tính tổng số ngày của dự án
            const totalDays = Math.ceil((projectEndDate - projectStartDate) / (1000 * 60 * 60 * 24)) + 1;
            
            // Chiều rộng của biểu đồ (trừ đi một khoảng cho phần tên công việc)
            const chartWidth = ganttChart.clientWidth;
            const dayWidth = chartWidth / totalDays * zoomLevel; // Apply zoom level
            
            // Chiều cao cho mỗi công việc
            const taskHeight = 30;
            
            // Chiều cao tổng của biểu đồ (dựa trên số lượng công việc)
            const chartHeight = tasks.length * taskHeight + 50; // +50 cho phần nhãn thời gian
            ganttChart.style.height = `${chartHeight}px`;
            
            // Xóa các nhãn thời gian cũ
            timeLabelsContainer.innerHTML = '';
            gridLinesContainer.innerHTML = '';
            
            // Tạo các nhãn thời gian và đường kẻ grid theo tháng
            let currentDate = new Date(projectStartDate);
            let monthCount = 0;
            
            while (currentDate <= projectEndDate) {
                const monthPosition = (monthCount * 30 * dayWidth);
                
                // Tạo nhãn tháng
                const monthLabel = document.createElement('div');
                monthLabel.className = 'gantt-time-label';
                monthLabel.style.left = `${monthPosition}px`;
                monthLabel.textContent = currentDate.toLocaleDateString('vi-VN', { month: 'numeric', year: 'numeric' });
                timeLabelsContainer.appendChild(monthLabel);
                
                // Tạo đường kẻ grid
                const gridLine = document.createElement('div');
                gridLine.className = 'gantt-grid-line';
                gridLine.style.left = `${monthPosition}px`;
                gridLinesContainer.appendChild(gridLine);
                
                // Tăng tháng
                currentDate.setMonth(currentDate.getMonth() + 1);
                monthCount++;
            }
            
            // Hiển thị các công việc
            tasks.forEach((task, index) => {
                const taskStartDate = new Date(task.startDate);
                const taskEndDate = new Date(task.endDate);
                
                // Tính vị trí và độ rộng của thanh công việc
                const daysFromStart = Math.ceil((taskStartDate - projectStartDate) / (1000 * 60 * 60 * 24));
                const taskDuration = Math.ceil((taskEndDate - taskStartDate) / (1000 * 60 * 60 * 24)) + 1;
                
                const taskLeft = daysFromStart * dayWidth;
                const taskWidth = taskDuration * dayWidth;
                const taskTop = index * taskHeight + 30; // +30 cho phần nhãn thời gian
                
                // Tạo thanh công việc
                const taskBar = document.createElement('div');
                taskBar.className = 'gantt-bar';
                taskBar.setAttribute('data-id', task.id);
                taskBar.setAttribute('data-name', task.name);
                taskBar.setAttribute('data-start', task.startDate);
                taskBar.setAttribute('data-end', task.endDate);
                taskBar.setAttribute('data-progress', task.progress);
                taskBar.setAttribute('data-status', task.status);
                
                if (task.status === "Hoàn thành") {
                    taskBar.classList.add('completed');
                } else if (task.status === "Đang thực hiện") {
                    taskBar.classList.add('in-progress');
                } else {
                    taskBar.classList.add('not-started');
                }
                
                if (task.critical && document.getElementById('show-critical-path').checked) {
                    taskBar.style.border = '2px solid #FF5722';
                }
                
                taskBar.style.left = `${taskLeft}px`;
                taskBar.style.top = `${taskTop}px`;
                taskBar.style.width = `${taskWidth}px`;
                
                // Tạo thanh tiến độ bên trong
                const progressBar = document.createElement('div');
                progressBar.className = 'progress-fill';
                progressBar.style.width = `${task.progress}%`;
                progressBar.style.height = '100%';
                
                // Thêm nhãn cho thanh công việc
                const taskLabel = document.createElement('div');
                taskLabel.className = 'gantt-bar-label';
                taskLabel.textContent = task.name;
                
                taskBar.appendChild(progressBar);
                taskBar.appendChild(taskLabel);
                
                // Thêm sự kiện hiện tooltip
                taskBar.addEventListener('mouseover', function(e) {
                    showTooltip(e, task);
                });
                
                taskBar.addEventListener('mousemove', function(e) {
                    moveTooltip(e);
                });
                
                taskBar.addEventListener('mouseout', function() {
                    hideTooltip();
                });
                
                barsContainer.appendChild(taskBar);
            });
            
            // Hiển thị các đợt thanh toán nếu được chọn
            if (document.getElementById('show-payments').checked) {
                payments.forEach(payment => {
                    const paymentDate = new Date(payment.date);
                    const daysFromStart = Math.ceil((paymentDate - projectStartDate) / (1000 * 60 * 60 * 24));
                    const paymentLeft = daysFromStart * dayWidth;
                    
                    // Tạo đánh dấu thanh toán
                    const paymentMarker = document.createElement('div');
                    paymentMarker.className = 'gantt-grid-line';
                    paymentMarker.style.left = `${paymentLeft}px`;
                    paymentMarker.style.borderLeft = '2px dashed #FFC107';
                    paymentMarker.style.zIndex = '5';
                    gridLinesContainer.appendChild(paymentMarker);
                    
                    // Tạo nhãn thanh toán
                    const paymentLabel = document.createElement('div');
                    paymentLabel.style.position = 'absolute';
                    paymentLabel.style.left = `${paymentLeft + 5}px`;
                    paymentLabel.style.top = '10px';
                    paymentLabel.style.color = '#FFC107';
                    paymentLabel.style.fontSize = '0.7rem';
                    paymentLabel.style.fontWeight = 'bold';
                    paymentLabel.textContent = `${payment.percent}%`;
                    paymentLabel.style.zIndex = '5';
                    barsContainer.appendChild(paymentLabel);
                });
            }
            
            // Hiển thị mối quan hệ phụ thuộc nếu được chọn
            if (document.getElementById('show-dependencies').checked) {
                drawDependencies(dependenciesContainer, dayWidth);
            }
        }
        
        function updateProjectInfo() {
            document.getElementById('project-name').textContent = projectData.name;
            document.getElementById('project-investor').textContent = projectData.investor;
            document.getElementById('project-contractor').textContent = projectData.contractor;
            document.getElementById('project-location').textContent = projectData.location;
            document.getElementById('project-start-date').textContent = formatDate(projectData.startDate);
            document.getElementById('project-end-date').textContent = formatDate(projectData.endDate);
            
            const startDate = new Date(projectData.startDate);
            const endDate = new Date(projectData.endDate);
            const months = (endDate.getFullYear() - startDate.getFullYear()) * 12 + 
                endDate.getMonth() - startDate.getMonth();
            const days = endDate.getDate() - startDate.getDate();
            
            document.getElementById('project-duration').textContent = `${months} tháng ${days > 0 ? days + ' ngày' : ''}`;
        }
        
        // Tooltip functions
        function showTooltip(event, task) {
            const tooltip = document.getElementById('gantt-tooltip');
            tooltip.style.display = 'block';
            tooltip.style.opacity = '1';
            
            // Set tooltip content
            tooltip.querySelector('.gantt-tooltip-title').textContent = task.name;
            document.getElementById('tooltip-start-date').textContent = formatDate(task.startDate);
            document.getElementById('tooltip-end-date').textContent = formatDate(task.endDate);
            document.getElementById('tooltip-duration').textContent = `${calculateDuration(task.startDate, task.endDate)} ngày`;
            document.getElementById('tooltip-progress').textContent = `${task.progress}%`;
            document.getElementById('tooltip-status').textContent = task.status;
            
            // Position the tooltip
            moveTooltip(event);
        }
        
        function moveTooltip(event) {
            const tooltip = document.getElementById('gantt-tooltip');
            const chartRect = document.querySelector('.gantt-chart-wrapper').getBoundingClientRect();
            
            // Calculate position - keep tooltip inside the chart area
            const offsetX = 10;
            const offsetY = 10;
            let x = event.clientX - chartRect.left + offsetX;
            let y = event.clientY - chartRect.top + offsetY;
            
            // Adjust position if tooltip would go outside the chart
            if (x + tooltip.offsetWidth > chartRect.width) {
                x = event.clientX - chartRect.left - tooltip.offsetWidth - offsetX;
            }
            
            if (y + tooltip.offsetHeight > chartRect.height) {
                y = event.clientY - chartRect.top - tooltip.offsetHeight - offsetY;
            }
            
            tooltip.style.left = `${x}px`;
            tooltip.style.top = `${y}px`;
        }
        
        function hideTooltip() {
            const tooltip = document.getElementById('gantt-tooltip');
            tooltip.style.opacity = '0';
            setTimeout(() => {
                tooltip.style.display = 'none';
            }, 200);
        }
        
        // Draw task dependencies
        function drawDependencies(container, dayWidth) {
            // For this example, we'll just illustrate some dependencies between tasks
            // In a real application, you would have a data structure defining these relationships
            
            // Example: Create dependencies between consecutive tasks where it makes sense
            // Parent tasks to their first child
            const parentTasks = tasks.filter(task => task.parentId === null);
            
            parentTasks.forEach(parent => {
                const children = tasks.filter(task => task.parentId === parent.id);
                if (children.length > 0) {
                    // Connect parent to first child
                    const firstChild = children[0];
                    drawConnection(container, parent, firstChild, dayWidth);
                    
                    // Connect children in sequence
                    for (let i = 0; i < children.length - 1; i++) {
                        drawConnection(container, children[i], children[i+1], dayWidth);
                    }
                }
            });
            
            // Connect the last child of one parent to the first child of the next parent
            for (let i = 0; i < parentTasks.length - 1; i++) {
                const currentParentChildren = tasks.filter(task => task.parentId === parentTasks[i].id);
                const nextParentChildren = tasks.filter(task => task.parentId === parentTasks[i+1].id);
                
                if (currentParentChildren.length > 0 && nextParentChildren.length > 0) {
                    const lastChild = currentParentChildren[currentParentChildren.length - 1];
                    const firstNextChild = nextParentChildren[0];
                    drawConnection(container, lastChild, firstNextChild, dayWidth);
                }
            }
        }
        
        function drawConnection(container, fromTask, toTask, dayWidth) {
            const projectStartDate = new Date(projectData.startDate);
            
            const fromTaskStartDate = new Date(fromTask.startDate);
            const fromTaskEndDate = new Date(fromTask.endDate);
            const toTaskStartDate = new Date(toTask.startDate);
            
            // Get task positions and dimensions
            const fromIndex = tasks.indexOf(fromTask);
            const toIndex = tasks.indexOf(toTask);
            
            const fromDaysFromStart = Math.ceil((fromTaskStartDate - projectStartDate) / (1000 * 60 * 60 * 24));
            const fromDuration = Math.ceil((fromTaskEndDate - fromTaskStartDate) / (1000 * 60 * 60 * 24)) + 1;
            
            const toDaysFromStart = Math.ceil((toTaskStartDate - projectStartDate) / (1000 * 60 * 60 * 24));
            
            const fromX = fromDaysFromStart * dayWidth + fromDuration * dayWidth;
            const fromY = fromIndex * 30 + 30 + 12.5; // +30 for time labels, +12.5 for middle of bar
            
            const toX = toDaysFromStart * dayWidth;
            const toY = toIndex * 30 + 30 + 12.5;
            
            // Create SVG path for the connection
            const svg = document.createElementNS("http://www.w3.org/2000/svg", "svg");
            svg.setAttribute("width", "100%");
            svg.setAttribute("height", "100%");
            svg.style.position = "absolute";
            svg.style.left = "0";
            svg.style.top = "0";
            svg.style.overflow = "visible";
            
            const path = document.createElementNS("http://www.w3.org/2000/svg", "path");
            
            // Calculate path d attribute for an elbow connector
            const midX = fromX + (toX - fromX) / 2;
            const pathData = `M${fromX},${fromY} L${midX},${fromY} L${midX},${toY} L${toX},${toY}`;
            
            path.setAttribute("d", pathData);
            path.setAttribute("class", "dependency-line");
            
            // Add arrow at the end
            const marker = document.createElementNS("http://www.w3.org/2000/svg", "marker");
            marker.setAttribute("id", `arrow-${fromTask.id}-${toTask.id}`);
            marker.setAttribute("viewBox", "0 0 10 10");
            marker.setAttribute("refX", "5");
            marker.setAttribute("refY", "5");
            marker.setAttribute("markerWidth", "6");
            marker.setAttribute("markerHeight", "6");
            marker.setAttribute("orient", "auto");
            
            const arrowPath = document.createElementNS("http://www.w3.org/2000/svg", "path");
            arrowPath.setAttribute("d", "M 0 0 L 10 5 L 0 10 z");
            arrowPath.setAttribute("class", "dependency-arrow");
            
            marker.appendChild(arrowPath);
            svg.appendChild(marker);
            
            path.setAttribute("marker-end", `url(#arrow-${fromTask.id}-${toTask.id})`);
            
            svg.appendChild(path);
            container.appendChild(svg);
        }
        
        // Modals
        function openEditProjectModal() {
            if (!isAuthenticated) {
                showNotification('Vui lòng đăng nhập để chỉnh sửa dữ liệu!', 'error');
                return;
            }
            
            document.getElementById('edit-project-name').value = projectData.name;
            document.getElementById('edit-project-investor').value = projectData.investor;
            document.getElementById('edit-project-contractor').value = projectData.contractor;
            document.getElementById('edit-project-location').value = projectData.location;
            document.getElementById('edit-project-start-date').value = projectData.startDate;
            document.getElementById('edit-project-end-date').value = projectData.endDate;
            document.getElementById('edit-project-total-value').value = projectData.totalValue;
            
            document.getElementById('editProjectModal').style.display = 'block';
        }
        
        function openTaskModal(taskId = null) {
            if (!isAuthenticated) {
                showNotification('Vui lòng đăng nhập để chỉnh sửa dữ liệu!', 'error');
                return;
            }
            
            document.getElementById('task-parent').innerHTML = '<option value="">Không có</option>';
            
            // Thêm các công việc cha vào dropdown
            const parentTasks = tasks.filter(task => task.parentId === null);
            parentTasks.forEach(task => {
                const option = document.createElement('option');
                option.value = task.id;
                option.textContent = task.name;
                document.getElementById('task-parent').appendChild(option);
            });
            
            if (taskId) {
                // Chỉnh sửa công việc hiện có
                const task = tasks.find(t => t.id === taskId);
                document.getElementById('taskModalTitle').textContent = 'Chỉnh sửa công việc';
                document.getElementById('task-id').value = task.id;
                document.getElementById('task-name').value = task.name;
                document.getElementById('task-parent').value = task.parentId || '';
                document.getElementById('task-start-date').value = task.startDate;
                document.getElementById('task-end-date').value = task.endDate;
                document.getElementById('task-progress').value = task.progress;
                document.getElementById('task-status').value = task.status;
                document.getElementById('task-assignee').value = task.assignee;
            } else {
                // Thêm công việc mới
                document.getElementById('taskModalTitle').textContent = 'Thêm công việc mới';
                document.getElementById('task-id').value = '';
                document.getElementById('task-name').value = '';
                document.getElementById('task-parent').value = '';
                document.getElementById('task-start-date').value = '';
                document.getElementById('task-end-date').value = '';
                document.getElementById('task-progress').value = 0;
                document.getElementById('task-status').value = 'Chưa bắt đầu';
                document.getElementById('task-assignee').value = '';
            }
            
            document.getElementById('taskModal').style.display = 'block';
        }
        
        function openPaymentModal(paymentId = null) {
            if (!isAuthenticated) {
                showNotification('Vui lòng đăng nhập để chỉnh sửa dữ liệu!', 'error');
                return;
            }
            
            if (paymentId) {
                // Chỉnh sửa đợt thanh toán hiện có
                const payment = payments.find(p => p.id === paymentId);
                document.getElementById('paymentModalTitle').textContent = 'Chỉnh sửa đợt thanh toán';
                document.getElementById('payment-id').value = payment.id;
                document.getElementById('payment-phase').value = payment.phase;
                document.getElementById('payment-percent').value = payment.percent;
                document.getElementById('payment-date').value = payment.date;
                document.getElementById('payment-status').value = payment.status;
            } else {
                // Thêm đợt thanh toán mới
                document.getElementById('paymentModalTitle').textContent = 'Thêm đợt thanh toán mới';
                document.getElementById('payment-id').value = '';
                document.getElementById('payment-phase').value = '';
                document.getElementById('payment-percent').value = '';
                document.getElementById('payment-date').value = '';
                document.getElementById('payment-status').value = 'Chưa đến hạn';
            }
            
            document.getElementById('paymentModal').style.display = 'block';
        }
        
        function openLoginModal() {
            document.getElementById('username').value = '';
            document.getElementById('password').value = '';
            document.getElementById('loginModal').style.display = 'block';
        }
        
        function openResetConfirmModal() {
            if (!isAuthenticated) {
                showNotification('Vui lòng đăng nhập để thực hiện chức năng này!', 'error');
                return;
            }
            
            document.getElementById('resetConfirmModal').style.display = 'block';
        }
        
        // Các hàm xử lý dữ liệu
        function editTask(taskId) {
            if (!isAuthenticated) {
                showNotification('Vui lòng đăng nhập để chỉnh sửa dữ liệu!', 'error');
                return;
            }
            
            openTaskModal(taskId);
        }
        
        function deleteTask(taskId) {
            if (!isAuthenticated) {
                showNotification('Vui lòng đăng nhập để chỉnh sửa dữ liệu!', 'error');
                return;
            }
            
            if (confirm('Bạn có chắc muốn xóa công việc này?')) {
                // Xóa các công việc con nếu là công việc cha
                tasks = tasks.filter(task => task.id !== taskId && task.parentId !== taskId);
                
                // Cập nhật giao diện
                updatePhases();
                renderTasksTable();
                renderTaskList();
                renderGanttChart();
                renderCriticalPathTable();
                renderPhasesTable();
                
                // Lưu dữ liệu
                triggerAutoSave();
            }
        }
        
        function moveTask(taskId, direction) {
            if (!isAuthenticated) {
                showNotification('Vui lòng đăng nhập để chỉnh sửa dữ liệu!', 'error');
                return;
            }
            
            // Tìm các công việc cha
            const parentTasks = tasks.filter(task => task.parentId === null);
            const taskIndex = parentTasks.findIndex(task => task.id === taskId);
            
            if (direction === 'up' && taskIndex > 0) {
                // Di chuyển lên
                [parentTasks[taskIndex], parentTasks[taskIndex - 1]] = [parentTasks[taskIndex - 1], parentTasks[taskIndex]];
            } else if (direction === 'down' && taskIndex < parentTasks.length - 1) {
                // Di chuyển xuống
                [parentTasks[taskIndex], parentTasks[taskIndex + 1]] = [parentTasks[taskIndex + 1], parentTasks[taskIndex]];
            }
            
            // Cập nhật mảng tasks
            const childTasks = tasks.filter(task => task.parentId !== null);
            tasks = [...parentTasks, ...childTasks];
            
            // Cập nhật giao diện
            updatePhases();
            renderTasksTable();
            renderTaskList();
            renderGanttChart();
            renderPhasesTable();
            
            // Lưu dữ liệu
            triggerAutoSave();
        }
        
        function editPayment(paymentId) {
            if (!isAuthenticated) {
                showNotification('Vui lòng đăng nhập để chỉnh sửa dữ liệu!', 'error');
                return;
            }
            
            openPaymentModal(paymentId);
        }
        
        function deletePayment(paymentId) {
            if (!isAuthenticated) {
                showNotification('Vui lòng đăng nhập để chỉnh sửa dữ liệu!', 'error');
                return;
            }
            
            if (confirm('Bạn có chắc muốn xóa đợt thanh toán này?')) {
                payments = payments.filter(payment => payment.id !== paymentId);
                
                // Cập nhật giao diện
                renderPaymentsTable();
                updateFinancialInfo();
                renderGanttChart();
                
                // Lưu dữ liệu
                triggerAutoSave();
            }
        }
        
        function saveTask(formData) {
            if (!isAuthenticated) {
                showNotification('Vui lòng đăng nhập để chỉnh sửa dữ liệu!', 'error');
                return;
            }
            
            if (formData.id) {
                // Cập nhật công việc hiện có
                const index = tasks.findIndex(task => task.id === parseInt(formData.id));
                tasks[index] = {
                    ...tasks[index],
                    name: formData.name,
                    parentId: formData.parentId ? parseInt(formData.parentId) : null,
                    startDate: formData.startDate,
                    endDate: formData.endDate,
                    progress: parseInt(formData.progress),
                    status: formData.status,
                    assignee: formData.assignee
                };
            } else {
                // Thêm công việc mới
                const newId = Math.max(...tasks.map(task => task.id)) + 1;
                tasks.push({
                    id: newId,
                    name: formData.name,
                    parentId: formData.parentId ? parseInt(formData.parentId) : null,
                    startDate: formData.startDate,
                    endDate: formData.endDate,
                    progress: parseInt(formData.progress),
                    status: formData.status,
                    assignee: formData.assignee,
                    critical: false
                });
            }
            
            // Cập nhật giao diện
            updatePhases();
            renderTasksTable();
            renderTaskList();
            renderGanttChart();
            renderCriticalPathTable();
            renderPhasesTable();
            
            // Lưu dữ liệu
            triggerAutoSave();
        }
        
        function savePayment(formData) {
            if (!isAuthenticated) {
                showNotification('Vui lòng đăng nhập để chỉnh sửa dữ liệu!', 'error');
                return;
            }
            
            if (formData.id) {
                // Cập nhật đợt thanh toán hiện có
                const index = payments.findIndex(payment => payment.id === parseInt(formData.id));
                payments[index] = {
                    ...payments[index],
                    phase: formData.phase,
                    percent: parseFloat(formData.percent),
                    date: formData.date,
                    status: formData.status
                };
            } else {
                // Thêm đợt thanh toán mới
                const newId = Math.max(...payments.map(payment => payment.id)) + 1;
                payments.push({
                    id: newId,
                    phase: formData.phase,
                    percent: parseFloat(formData.percent),
                    date: formData.date,
                    status: formData.status
                });
            }
            
            // Cập nhật giao diện
            renderPaymentsTable();
            updateFinancialInfo();
            renderGanttChart();
            
            // Lưu dữ liệu
            triggerAutoSave();
        }
        
        function saveProjectInfo(formData) {
            if (!isAuthenticated) {
                showNotification('Vui lòng đăng nhập để chỉnh sửa dữ liệu!', 'error');
                return;
            }
            
            projectData = {
                ...projectData,
                name: formData.name,
                investor: formData.investor,
                contractor: formData.contractor,
                location: formData.location,
                startDate: formData.startDate,
                endDate: formData.endDate,
                totalValue: parseInt(formData.totalValue)
            };
            
            // Cập nhật giao diện
            updateProjectInfo();
            updateFinancialInfo();
            renderGanttChart();
            
            // Lưu dữ liệu
            triggerAutoSave();
        }
        
        // Zoom Controls
        function zoomIn() {
            if (zoomLevel < 3) {
                zoomLevel += 0.25;
                renderGanttChart();
            }
        }
        
        function zoomOut() {
            if (zoomLevel > 0.5) {
                zoomLevel -= 0.25;
                renderGanttChart();
            }
        }
        
        function resetZoom() {
            zoomLevel = 1;
            renderGanttChart();
            
            // Reset scroll position
            document.querySelector('.gantt-chart-wrapper').scrollLeft = 0;
        }
        
        // Xuất dữ liệu
        function exportToExcel() {
            // Tạo dữ liệu cho file Excel
            const workbook = XLSX.utils.book_new();
            
            // Thông tin dự án
            const projectInfo = [
                ["Quản Lý Tiến Độ Dự Án"],
                [""],
                ["Thông tin dự án"],
                ["Tên dự án", projectData.name],
                ["Chủ đầu tư", projectData.investor],
                ["Đơn vị thi công", projectData.contractor],
                ["Địa điểm", projectData.location],
                ["Ngày khởi công", formatDate(projectData.startDate)],
                ["Dự kiến hoàn thành", formatDate(projectData.endDate)],
                ["Tổng thời gian", document.getElementById('project-duration').textContent],
                [""],
                ["Thông tin tài chính"],
                ["Tổng giá trị dự án", formatCurrency(projectData.totalValue)],
                ["Đã thanh toán", document.getElementById('project-paid-value').textContent],
                ["Còn lại", document.getElementById('project-remaining-value').textContent],
                ["Tiến độ thanh toán", `${projectData.paidPercent.toFixed(0)}%`]
            ];
            
            // Tiến độ theo giai đoạn
            const phasesData = [
                [""],
                ["Tiến độ theo giai đoạn"],
                ["Giai đoạn", "Thời gian", "Tiến độ", "Trạng thái"]
            ];
            
            phases.forEach(phase => {
                phasesData.push([
                    phase.name,
                    `${formatDate(phase.startDate)} - ${formatDate(phase.endDate)}`,
                    `${phase.progress}%`,
                    phase.status
                ]);
            });
            
            // Chi tiết công việc
            const tasksData = [
                [""],
                ["Chi tiết công việc"],
                ["ID", "Công việc", "Ngày bắt đầu", "Ngày kết thúc", "Thời lượng", "Tiến độ", "Trạng thái", "Người phụ trách"]
            ];
            
            tasks.forEach(task => {
                tasksData.push([
                    task.id,
                    task.parentId ? `    ${task.name}` : task.name,
                    formatDate(task.startDate),
                    formatDate(task.endDate),
                    `${calculateDuration(task.startDate, task.endDate)} ngày`,
                    `${task.progress}%`,
                    task.status,
                    task.assignee
                ]);
            });
            
            // Đợt thanh toán
            const paymentsData = [
                [""],
                ["Đợt thanh toán"],
                ["STT", "Giai đoạn", "Giá trị (%)", "Giá trị (VNĐ)", "Thời gian", "Trạng thái"]
            ];
            
            payments.forEach((payment, index) => {
                paymentsData.push([
                    index + 1,
                    payment.phase,
                    `${payment.percent}%`,
                    formatCurrency((payment.percent / 100) * projectData.totalValue),
                    formatDate(payment.date),
                    payment.status
                ]);
            });
            
            // Gộp tất cả dữ liệu
            const allData = [...projectInfo, ...phasesData, ...tasksData, ...paymentsData];
            
            // Tạo worksheet và thêm vào workbook
            const worksheet = XLSX.utils.aoa_to_sheet(allData);
            XLSX.utils.book_append_sheet(workbook, worksheet, "Quản lý tiến độ");
            
            // Tạo file Excel và tải xuống
            XLSX.writeFile(workbook, `Quản_lý_tiến_độ_${projectData.name.replace(/ /g, "_")}.xlsx`);
        }
        
        function exportToPDF() {
            // Ẩn các nút không cần thiết khi xuất PDF
            const buttonsToHide = document.querySelectorAll('button');
            buttonsToHide.forEach(button => {
                button.style.display = 'none';
            });
            
            // Hiển thị tất cả tab content
            const tabContents = document.querySelectorAll('.tab-content');
            tabContents.forEach(content => {
                content.style.display = 'block';
            });
            
            // Ẩn tabs
            document.querySelector('.tabs').style.display = 'none';
            
            // Ẩn các phần điều khiển zoom
            document.querySelector('.gantt-zoom-controls').style.display = 'none';
            
            // Ẩn thông tin đăng nhập
            document.getElementById('loginContainer').style.display = 'none';
            document.getElementById('userInfoContainer').style.display = 'none';
            
            // Đặt lại zoom về mức bình thường
            const currentZoom = zoomLevel;
            zoomLevel = 1;
            renderGanttChart();
            
            // Tạo PDF
            const element = document.querySelector('.container');
            const opt = {
                margin: 10,
                filename: `Quản_lý_tiến_độ_${projectData.name.replace(/ /g, "_")}.pdf`,
                image: { type: 'jpeg', quality: 0.98 },
                html2canvas: { scale: 2 },
                jsPDF: { unit: 'mm', format: 'a4', orientation: 'portrait' }
            };
            
            html2pdf().from(element).set(opt).save().then(() => {
                // Khôi phục hiển thị sau khi xuất PDF
                buttonsToHide.forEach(button => {
                    button.style.display = 'inline-block';
                });
                
                tabContents.forEach(content => {
                    content.style.display = 'none';
                });
                
                document.querySelector('.tabs').style.display = 'flex';
                document.querySelector('.gantt-zoom-controls').style.display = 'flex';
                
                // Khôi phục hiển thị thông tin đăng nhập
                if (isAuthenticated) {
                    document.getElementById('userInfoContainer').style.display = 'flex';
                } else {
                    document.getElementById('loginContainer').style.display = 'block';
                }
                
                document.getElementById('overview').style.display = 'block';
                
                // Khôi phục zoom level
                zoomLevel = currentZoom;
                renderGanttChart();
                
                // Khôi phục tab đang active
                const activeTab = document.querySelector('.tab.active').getAttribute('data-tab');
                document.getElementById(activeTab).style.display = 'block';
                
                // Cập nhật UI dựa trên trạng thái đăng nhập
                updateUIBasedOnAuth();
            });
        }
        
        // Hàm lưu và tải dữ liệu
        function saveDataToLocalStorage() {
            if (!isAuthenticated) {
                showNotification('Vui lòng đăng nhập để lưu dữ liệu!', 'error');
                return false;
            }
            
            const data = {
                projectData: projectData,
                tasks: tasks,
                payments: payments,
                phases: phases,
                lastSaved: new Date().toISOString()
            };
            
            localStorage.setItem('projectManagementData', JSON.stringify(data));
            
            // Cập nhật thời gian lưu cuối cùng
            updateLastSavedTime(data.lastSaved);
            
            return true;
        }
        
        function loadDataFromLocalStorage() {
            const data = localStorage.getItem('projectManagementData');
            if (!data) return false;
            
            try {
                const parsedData = JSON.parse(data);
                
                projectData = parsedData.projectData;
                tasks = parsedData.tasks;
                payments = parsedData.payments;
                phases = parsedData.phases || [];
                
                // Cập nhật giao diện
                updateProjectInfo();
                updateFinancialInfo();
                updatePhases();
                renderTasksTable();
                renderPaymentsTable();
                renderPhasesTable();
                renderTaskList();
                renderGanttChart();
                renderCriticalPathTable();
                
                // Cập nhật thời gian lưu cuối cùng
                updateLastSavedTime(parsedData.lastSaved);
                
                return true;
            } catch (error) {
                console.error('Error loading data:', error);
                return false;
            }
        }
        
        function triggerAutoSave() {
            // Kiểm tra trạng thái đăng nhập
            if (!isAuthenticated) return;
            
            // Hiển thị đang lưu
            showAutoSaveIndicator(true);
            
            // Hủy timeout hiện tại nếu có
            if (autoSaveTimeout) {
                clearTimeout(autoSaveTimeout);
            }
            
            // Đặt timeout mới để lưu sau 2 giây không hoạt động
            autoSaveTimeout = setTimeout(() => {
                saveDataToLocalStorage();
                showNotification('Dữ liệu đã được tự động lưu', 'success');
                showAutoSaveIndicator(false);
            }, 2000);
        }
        
        function showAutoSaveIndicator(show) {
            const indicator = document.getElementById('autosaveIndicator');
            if (show) {
                indicator.classList.add('active');
            } else {
                indicator.classList.remove('active');
            }
        }
        
        function updateLastSavedTime(timeString) {
            const time = new Date(timeString);
            const formattedTime = time.toLocaleTimeString('vi-VN') + ' ' + time.toLocaleDateString('vi-VN');
            document.getElementById('lastSaved').textContent = `Đã lưu lần cuối lúc: ${formattedTime}`;
        }
        
        function showNotification(message, type) {
            const notification = document.getElementById('notification');
            notification.textContent = message;
            notification.className = `notification ${type}`;
            notification.classList.add('show');
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 3000);
        }
        
        // Event Listeners và khởi tạo
        document.addEventListener('DOMContentLoaded', function() {
            // Khởi tạo hệ thống authentication
            initializeAuth();
            
            // Tải dữ liệu từ localStorage nếu có
            const dataLoaded = loadDataFromLocalStorage();
            if (dataLoaded) {
                showNotification('Đã tải dữ liệu từ lần làm việc trước', 'info');
            }
            
            // Tab navigation
            document.querySelectorAll('.tab').forEach(tab => {
                tab.addEventListener('click', function() {
                    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
                    document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
                    
                    this.classList.add('active');
                    document.getElementById(this.getAttribute('data-tab')).classList.add('active');
                    
                    // Cập nhật biểu đồ Gantt khi chuyển tab
                    if (this.getAttribute('data-tab') === 'gantt') {
                        renderTaskList();
                        renderGanttChart();
                    }
                });
            });
            
            // Nút lưu dữ liệu
            document.getElementById('saveDataBtn').addEventListener('click', function() {
                if (!isAuthenticated) {
                    showNotification('Vui lòng đăng nhập để lưu dữ liệu!', 'error');
                    return;
                }
                
                if (saveDataToLocalStorage()) {
                    showNotification('Dữ liệu đã được lưu thành công', 'success');
                } else {
                    showNotification('Lỗi khi lưu dữ liệu', 'error');
                }
            });
            
            // Nút reset dữ liệu
            document.getElementById('resetDataBtn').addEventListener('click', function() {
                openResetConfirmModal();
            });
            
            // Xác nhận reset dữ liệu
            document.getElementById('confirmResetBtn').addEventListener('click', function() {
                resetData();
                document.getElementById('resetConfirmModal').style.display = 'none';
            });
            
            // Hủy reset dữ liệu
            document.getElementById('cancelResetBtn').addEventListener('click', function() {
                document.getElementById('resetConfirmModal').style.display = 'none';
            });
            
            // Đóng modal xác nhận reset
            document.getElementById('closeResetConfirmModal').addEventListener('click', function() {
                document.getElementById('resetConfirmModal').style.display = 'none';
            });
            
            // Zoom controls
            document.getElementById('zoom-in').addEventListener('click', zoomIn);
            document.getElementById('zoom-out').addEventListener('click', zoomOut);
            document.getElementById('zoom-reset').addEventListener('click', resetZoom);
            
            // Mở modal chỉnh sửa thông tin dự án
            document.getElementById('editProjectBtn').addEventListener('click', openEditProjectModal);
            
            // Mở modal thêm công việc
            document.getElementById('addTaskBtn').addEventListener('click', () => openTaskModal());
            
            // Mở modal thêm đợt thanh toán
            document.getElementById('addPaymentBtn').addEventListener('click', () => openPaymentModal());
            document.getElementById('addPaymentBtn2').addEventListener('click', () => openPaymentModal());
            
            // Mở modal đăng nhập
            document.getElementById('loginBtn').addEventListener('click', openLoginModal);
            
            // Đăng xuất
            document.getElementById('logoutBtn').addEventListener('click', logout);
            
            // Đóng modal
            document.getElementById('closeEditProjectModal').addEventListener('click', () => {
                document.getElementById('editProjectModal').style.display = 'none';
            });
            
            document.getElementById('closeTaskModal').addEventListener('click', () => {
                document.getElementById('taskModal').style.display = 'none';
            });
            
            document.getElementById('closePaymentModal').addEventListener('click', () => {
                document.getElementById('paymentModal').style.display = 'none';
            });
            
            document.getElementById('closeLoginModal').addEventListener('click', () => {
                document.getElementById('loginModal').style.display = 'none';
            });
            
            // Xử lý form đăng nhập
            document.getElementById('loginForm').addEventListener('submit', function(e) {
                e.preventDefault();
                
                const username = document.getElementById('username').value;
                const password = document.getElementById('password').value;
                
                login(username, password);
            });
            
            // Lưu form
            document.getElementById('editProjectForm').addEventListener('submit', function(e) {
                e.preventDefault();
                
                const formData = {
                    name: document.getElementById('edit-project-name').value,
                    investor: document.getElementById('edit-project-investor').value,
                    contractor: document.getElementById('edit-project-contractor').value,
                    location: document.getElementById('edit-project-location').value,
                    startDate: document.getElementById('edit-project-start-date').value,
                    endDate: document.getElementById('edit-project-end-date').value,
                    totalValue: document.getElementById('edit-project-total-value').value
                };
                
                saveProjectInfo(formData);
                document.getElementById('editProjectModal').style.display = 'none';
            });
            
            document.getElementById('taskForm').addEventListener('submit', function(e) {
                e.preventDefault();
                
                const formData = {
                    id: document.getElementById('task-id').value,
                    name: document.getElementById('task-name').value,
                    parentId: document.getElementById('task-parent').value,
                    startDate: document.getElementById('task-start-date').value,
                    endDate: document.getElementById('task-end-date').value,
                    progress: document.getElementById('task-progress').value,
                    status: document.getElementById('task-status').value,
                    assignee: document.getElementById('task-assignee').value
                };
                
                saveTask(formData);
                document.getElementById('taskModal').style.display = 'none';
            });
            
            document.getElementById('paymentForm').addEventListener('submit', function(e) {
                e.preventDefault();
                
                const formData = {
                    id: document.getElementById('payment-id').value,
                    phase: document.getElementById('payment-phase').value,
                    percent: document.getElementById('payment-percent').value,
                    date: document.getElementById('payment-date').value,
                    status: document.getElementById('payment-status').value
                };
                
                savePayment(formData);
                document.getElementById('paymentModal').style.display = 'none';
            });
            
            // Xuất file
            document.getElementById('exportExcel').addEventListener('click', exportToExcel);
            document.getElementById('exportPDF').addEventListener('click', exportToPDF);
            
            // Filter controls
            document.getElementById('show-critical-path').addEventListener('change', renderGanttChart);
            document.getElementById('show-dependencies').addEventListener('change', renderGanttChart);
            document.getElementById('show-payments').addEventListener('change', renderGanttChart);
            
            // Window resize
            window.addEventListener('resize', function() {
                if (document.querySelector('.tab.active').getAttribute('data-tab') === 'gantt') {
                    renderGanttChart();
                }
            });
            
            // Thêm auto-save khi giá trị thay đổi
            document.querySelectorAll('input, select, textarea').forEach(element => {
                element.addEventListener('change', function() {
                    if (isAuthenticated) {
                        triggerAutoSave();
                    }
                });
            });
            
            // Lưu dữ liệu trước khi đóng trang
            window.addEventListener('beforeunload', function() {
                if (isAuthenticated) {
                    saveDataToLocalStorage();
                }
            });
            
            // Khởi tạo dữ liệu nếu chưa tải từ localStorage
            if (!dataLoaded) {
                updatePhases(); // Tạo phases từ tasks
                updateProjectInfo();
                updateFinancialInfo();
                renderPhasesTable();
                renderTasksTable();
                renderPaymentsTable();
                renderCriticalPathTable();
            }
            
            // Khởi tạo biểu đồ Gantt nếu tab Gantt là tab active
            if (document.querySelector('.tab.active').getAttribute('data-tab') === 'gantt') {
                renderTaskList();
                renderGanttChart();
            }
            
            // Cập nhật UI dựa trên trạng thái đăng nhập
            updateUIBasedOnAuth();
        });
    </script>
</body>
</html>
   
