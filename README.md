<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Instagram Analytics Dashboard</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/2.0.5/FileSaver.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background-color: #fafafa;
            color: #262626;
        }
        
        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 0 20px;
        }
        
        /* Header Styles */
        header {
            background-color: #fff;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
            position: sticky;
            top: 0;
            z-index: 100;
        }
        
        .header-content {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 0;
        }
        
        .logo {
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 24px;
            font-weight: 700;
            color: #405DE6;
        }
        
        .logo i {
            color: #E1306C;
        }
        
        nav ul {
            display: flex;
            list-style: none;
            gap: 30px;
        }
        
        nav a {
            text-decoration: none;
            color: #262626;
            font-weight: 500;
            transition: color 0.3s;
        }
        
        nav a:hover, nav a.active {
            color: #405DE6;
        }
        
        .auth-buttons {
            display: flex;
            gap: 15px;
        }
        
        .btn {
            padding: 8px 20px;
            border-radius: 8px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .btn-outline {
            border: 1px solid #405DE6;
            color: #405DE6;
            background: transparent;
        }
        
        .btn-primary {
            background: linear-gradient(45deg, #405DE6, #5851DB, #833AB4, #C13584, #E1306C, #FD1D1D);
            color: white;
            border: none;
        }
        
        /* Login Section */
        .login-section {
            background-color: #fff;
            padding: 40px;
            margin: 40px 0;
            border-radius: 12px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
            text-align: center;
            max-width: 500px;
            margin-left: auto;
            margin-right: auto;
        }
        
        .login-title {
            font-size: 24px;
            margin-bottom: 20px;
            color: #262626;
        }
        
        .login-form {
            display: flex;
            flex-direction: column;
            gap: 20px;
            margin-top: 30px;
        }
        
        .form-group {
            display: flex;
            flex-direction: column;
            text-align: left;
            gap: 8px;
        }
        
        .form-group label {
            font-weight: 600;
            color: #262626;
        }
        
        .form-group input {
            padding: 12px 15px;
            border: 1px solid #dbdbdb;
            border-radius: 8px;
            font-size: 16px;
        }
        
        .login-btn {
            background: linear-gradient(45deg, #405DE6, #5851DB, #833AB4, #C13584, #E1306C, #FD1D1D);
            color: white;
            border: none;
            padding: 12px 30px;
            border-radius: 30px;
            font-weight: 600;
            cursor: pointer;
            margin-top: 15px;
            transition: all 0.3s;
        }
        
        .login-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
        }
        
        .divider {
            display: flex;
            align-items: center;
            margin: 20px 0;
        }
        
        .divider span {
            padding: 0 15px;
            color: #8e8e8e;
        }
        
        .divider::before, .divider::after {
            content: '';
            flex: 1;
            height: 1px;
            background-color: #dbdbdb;
        }
        
        .instagram-login {
            background: linear-gradient(45deg, #405DE6, #5851DB, #833AB4, #C13584, #E1306C, #FD1D1D);
            color: white;
            border: none;
            padding: 12px 30px;
            border-radius: 30px;
            font-weight: 600;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            margin: 0 auto;
            transition: all 0.3s;
        }
        
        .instagram-login:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
        }
        
        /* Data Upload Section */
        .data-upload-section {
            background-color: #fff;
            padding: 40px;
            margin: 20px 0;
            border-radius: 12px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
            text-align: center;
        }
        
        .upload-title {
            font-size: 24px;
            margin-bottom: 20px;
            color: #262626;
        }
        
        .upload-area {
            border: 2px dashed #dbdbdb;
            border-radius: 12px;
            padding: 40px;
            margin-bottom: 20px;
            transition: all 0.3s;
        }
        
        .upload-area:hover {
            border-color: #405DE6;
        }
        
        .upload-icon {
            font-size: 48px;
            color: #405DE6;
            margin-bottom: 15px;
        }
        
        .upload-text {
            color: #8e8e8e;
            margin-bottom: 10px;
        }
        
        .upload-btn {
            background: linear-gradient(45deg, #405DE6, #5851DB, #833AB4, #C13584, #E1306C, #FD1D1D);
            color: white;
            border: none;
            padding: 12px 30px;
            border-radius: 30px;
            font-weight: 600;
            cursor: pointer;
            margin-top: 15px;
            transition: all 0.3s;
        }
        
        .upload-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
        }
        
        .file-info {
            margin: 15px 0;
            padding: 10px;
            border-radius: 8px;
            background-color: #fafafa;
        }
        
        .data-status {
            padding: 10px;
            border-radius: 8px;
            margin: 10px 0;
        }
        
        .status-success {
            background-color: #d4edda;
            color: #155724;
        }
        
        .status-error {
            background-color: #f8d7da;
            color: #721c24;
        }
        
        /* Dashboard Tabs */
        .dashboard-tabs {
            display: flex;
            background-color: #fff;
            border-radius: 12px 12px 0 0;
            overflow-x: auto;
            margin-top: 40px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
        }
        
        .tab {
            padding: 15px 25px;
            cursor: pointer;
            font-weight: 600;
            white-space: nowrap;
            border-bottom: 3px solid transparent;
            transition: all 0.3s;
        }
        
        .tab.active {
            border-bottom: 3px solid #405DE6;
            color: #405DE6;
        }
        
        .tab-content {
            display: none;
            background-color: #fff;
            padding: 30px;
            border-radius: 0 0 12px 12px;
            margin-bottom: 40px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
        }
        
        .tab-content.active {
            display: block;
        }
        
        /* Stats Grid */
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin: 30px 0;
        }
        
        .stat-card {
            background-color: #fafafa;
            border-radius: 12px;
            padding: 20px;
            text-align: center;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
        }
        
        .stat-icon {
            font-size: 24px;
            color: #405DE6;
            margin-bottom: 10px;
        }
        
        .stat-value {
            font-size: 28px;
            font-weight: 700;
            margin-bottom: 5px;
        }
        
        .stat-label {
            color: #8e8e8e;
            font-size: 14px;
        }
        
        /* Charts */
        .chart-container {
            margin: 30px 0;
            position: relative;
            height: 300px;
        }
        
        .chart-row {
            display: flex;
            gap: 20px;
            margin: 30px 0;
        }
        
        .chart-half {
            flex: 1;
            min-width: 0;
        }
        
        /* Heatmap Styles */
        .heatmap-container {
            margin: 30px 0;
        }
        
        .heatmap {
            display: grid;
            grid-template-columns: repeat(24, 1fr);
            gap: 2px;
            margin-top: 15px;
        }
        
        .heatmap-hour {
            height: 20px;
            border-radius: 2px;
            position: relative;
        }
        
        .heatmap-hour.low {
            background-color: #e0f2f1;
        }
        
        .heatmap-hour.medium {
            background-color: #80cbc4;
        }
        
        .heatmap-hour.high {
            background-color: #26a69a;
        }
        
        .heatmap-hour.very-high {
            background-color: #00897b;
        }
        
        .heatmap-hour.extreme {
            background-color: #00695c;
        }
        
        .heatmap-hour:hover::after {
            content: attr(data-tooltip);
            position: absolute;
            bottom: 100%;
            left: 50%;
            transform: translateX(-50%);
            background-color: #333;
            color: white;
            padding: 4px 8px;
            border-radius: 4px;
            font-size: 12px;
            white-space: nowrap;
            z-index: 100;
        }
        
        .heatmap-days {
            display: grid;
            grid-template-rows: repeat(7, 1fr);
            gap: 2px;
            margin-right: 10px;
        }
        
        .heatmap-day {
            height: 20px;
            display: flex;
            align-items: center;
            font-size: 12px;
        }
        
        .heatmap-wrapper {
            display: flex;
        }
        
        .heatmap-hours {
            display: grid;
            grid-template-columns: repeat(24, 1fr);
            gap: 2px;
            margin-bottom: 10px;
        }
        
        .heatmap-hour-label {
            font-size: 10px;
            text-align: center;
            transform: rotate(-45deg);
            height: 40px;
        }
        
        /* Competitor Comparison */
        .competitor-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin: 30px 0;
        }
        
        .competitor-card {
            background-color: #fafafa;
            border-radius: 12px;
            padding: 20px;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
        }
        
        .competitor-header {
            display: flex;
            align-items: center;
            margin-bottom: 15px;
        }
        
        .competitor-avatar {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            margin-right: 15px;
            background-color: #ddd;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
        }
        
        .competitor-name {
            font-weight: 600;
            font-size: 18px;
        }
        
        .competitor-stats {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
        }
        
        .competitor-stat {
            text-align: center;
            padding: 10px;
        }
        
        .competitor-stat-value {
            font-size: 20px;
            font-weight: 700;
            margin-bottom: 5px;
        }
        
        .competitor-stat-label {
            color: #8e8e8e;
            font-size: 12px;
        }
        
        /* Hashtag Performance */
        .hashtag-list {
            margin: 20px 0;
        }
        
        .hashtag-item {
            display: flex;
            justify-content: space-between;
            padding: 10px 0;
            border-bottom: 1px solid #eee;
        }
        
        .hashtag-name {
            font-weight: 600;
        }
        
        .hashtag-stats {
            display: flex;
            gap: 15px;
        }
        
        .hashtag-stat {
            text-align: center;
            min-width: 80px;
        }
        
        .hashtag-stat-value {
            font-weight: 600;
            color: #405DE6;
        }
        
        .hashtag-stat-label {
            font-size: 12px;
            color: #8e8e8e;
        }
        
        /* Control Buttons */
        .control-buttons {
            display: flex;
            gap: 15px;
            margin: 20px 0;
            flex-wrap: wrap;
        }
        
        .control-btn {
            background-color: #f1f1f1;
            border: none;
            padding: 10px 20px;
            border-radius: 8px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .control-btn:hover {
            background-color: #e0e0e0;
        }
        
        .back-btn {
            background-color: #405DE6;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 8px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            gap: 8px;
            margin-bottom: 20px;
        }
        
        .back-btn:hover {
            background-color: #304ac7;
        }
        
        /* Raw Data Modal */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }
        
        .modal-content {
            background-color: white;
            padding: 30px;
            border-radius: 12px;
            max-width: 800px;
            width: 90%;
            max-height: 80vh;
            overflow: auto;
        }
        
        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        
        .close-modal {
            background: none;
            border: none;
            font-size: 24px;
            cursor: pointer;
        }
        
        pre {
            background-color: #fafafa;
            padding: 15px;
            border-radius: 8px;
            overflow: auto;
            max-height: 60vh;
        }
        
        /* Footer */
        footer {
            background-color: #fff;
            padding: 40px 0;
            margin-top: 60px;
            box-shadow: 0 -2px 4px rgba(0, 0, 0, 0.05);
        }
        
        .footer-content {
            display: flex;
            justify-content: space-between;
            flex-wrap: wrap;
            gap: 40px;
        }
        
        .footer-column {
            flex: 1;
            min-width: 200px;
        }
        
        .footer-column h3 {
            margin-bottom: 20px;
            color: #262626;
        }
        
        .footer-column ul {
            list-style: none;
        }
        
        .footer-column li {
            margin-bottom: 10px;
        }
        
        .footer-column a {
            text-decoration: none;
            color: #8e8e8e;
            transition: color 0.3s;
        }
        
        .footer-column a:hover {
            color: #405DE6;
        }
        
        .copyright {
            text-align: center;
            margin-top: 40px;
            color: #8e8e8e;
            font-size: 14px;
        }
        
        /* Responsive Design */
        @media (max-width: 768px) {
            .header-content {
                flex-direction: column;
                gap: 15px;
            }
            
            nav ul {
                gap: 15px;
                flex-wrap: wrap;
                justify-content: center;
            }
            
            .upload-area {
                padding: 20px;
            }
            
            .dashboard-tabs {
                flex-wrap: nowrap;
                overflow-x: auto;
            }
            
            .stats-grid {
                grid-template-columns: 1fr;
            }
            
            .competitor-grid {
                grid-template-columns: 1fr;
            }
            
            .control-buttons {
                flex-direction: column;
            }
            
            .login-section {
                padding: 20px;
            }
            
            .heatmap {
                grid-template-columns: repeat(12, 1fr);
            }
            
            .heatmap-hours {
                grid-template-columns: repeat(12, 1fr);
            }
            
            .chart-row {
                flex-direction: column;
            }
        }
        
        /* Hidden class for toggling visibility */
        .hidden {
            display: none;
        }
    </style>
</head>
<body>
    <header>
        <div class="container">
            <div class="header-content">
                <div class="logo">
                    <i class="fab fa-instagram"></i>
                    <span>Instagram Analytics</span>
                </div>
                
                <nav>
                    <ul>
                        <li><a href="#" class="active">Dashboard</a></li>
                    </ul>
                </nav>
                
                <div class="auth-buttons">
                    <button class="btn btn-outline" id="logoutBtn">Log Out</button>
                </div>
            </div>
        </div>
    </header>
    
    <main class="container">
        <section class="login-section" id="loginSection">
            <div class="login-title">Login to Instagram Analytics</div>
            <p>Enter your credentials to access advanced analytics for your Instagram account</p>
            
            <div class="login-form">
                <div class="form-group">
                    <label for="username">Username</label>
                    <input type="text" id="username" placeholder="Your Instagram username">
                </div>
                
                <div class="form-group">
                    <label for="password">Password</label>
                    <input type="password" id="password" placeholder="Your Instagram password">
                </div>
                
                <button class="login-btn" onclick="login()">Log In</button>
                
                <div class="divider">
                    <span>OR</span>
                </div>
                
                <button class="instagram-login" onclick="loginWithInstagram()">
                    <i class="fab fa-instagram"></i> Log in with Instagram
                </button>
            </div>
        </section>
        
        <div id="analyticsDashboard" class="hidden">
            <button class="back-btn" onclick="showLoginSection()">
                <i class="fas fa-arrow-left"></i> Back to Login
            </button>
            
            <section class="data-upload-section">
                <div class="upload-title">Analyze Instagram Profiles</div>
                <div class="upload-area" id="uploadBox">
                    <div class="upload-icon"><i class="fab fa-instagram"></i></div>
                    <div class="upload-text">You are logged in as: <span id="loggedInUser">user123</span></div>
                    <div class="upload-text">Enter a username to analyze profile data</div>
                    <input type="text" id="profileUsername" placeholder="Instagram username" style="padding: 10px; width: 80%; max-width: 300px; margin: 10px 0; border-radius: 8px; border: 1px solid #dbdbdb;">
                    <button class="upload-btn" onclick="analyzeProfile()">Analyze Profile</button>
                </div>
                <div class="file-info" id="fileInfo"></div>
                <div id="dataStatus" class="data-status"></div>
                
                <div class="control-buttons">
                    <button class="control-btn" onclick="fetchRealData()"><i class="fas fa-sync"></i> Refresh Data</button>
                    <button class="control-btn" onclick="exportToPDF()"><i class="fas fa-file-pdf"></i> Export PDF</button>
                    <button class="control-btn" onclick="viewRawData()"><i class="fas fa-database"></i> View Raw Data</button>
                </div>
            </section>
            
            <div class="dashboard-tabs">
                <div class="tab active" onclick="switchTab('overview')">Overview</div>
                <div class="tab" onclick="switchTab('engagement')">Engagement</div>
                <div class="tab" onclick="switchTab('audience')">Audience</div>
                <div class="tab" onclick="switchTab('content')">Content</div>
                <div class="tab" onclick="switchTab('growth')">Growth</div>
                <div class="tab" onclick="switchTab('competitors')">Competitors</div>
            </div>
            
            <div id="overview" class="tab-content active">
                <h2>Account Overview</h2>
                <p>Summary of your Instagram account performance</p>
                
                <div class="stats-grid">
                    <div class="stat-card">
                        <div class="stat-icon"><i class="fas fa-users"></i></div>
                        <div class="stat-value" id="followersCount">0</div>
                        <div class="stat-label">Followers</div>
                    </div>
                    
                    <div class="stat-card">
                        <div class="stat-icon"><i class="fas fa-user-plus"></i></div>
                        <div class="stat-value" id="followingCount">0</div>
                        <div class="stat-label">Following</div>
                    </div>
                    
                    <div class="stat-card">
                        <div class="stat-icon"><i class="fas fa-image"></i></div>
                        <div class="stat-value" id="postsCount">0</div>
                        <div class="stat-label">Posts</div>
                    </div>
                    
                    <div class="stat-card">
                        <div class="stat-icon"><i class="fas fa-heart"></i></div>
                        <div class="stat-value" id="avgLikes">0</div>
                        <div class="stat-label">Avg. Likes</div>
                    </div>
                    
                    <div class="stat-card">
                        <div class="stat-icon"><i class="fas fa-comment"></i></div>
                        <div class="stat-value" id="avgComments">0</div>
                        <div class="stat-label">Avg. Comments</div>
                    </div>
                    
                    <div class="stat-card">
                        <div class="stat-icon"><i class="fas fa-percentage"></i></div>
                        <div class="stat-value" id="engagementRate">0%</div>
                        <div class="stat-label">Engagement Rate</div>
                    </div>
                </div>
                
                <h3>Follower Growth</h3>
                <div class="chart-container">
                    <canvas id="growthChart"></canvas>
                </div>
            </div>
            
            <div id="engagement" class="tab-content">
                <h2>Engagement Analytics</h2>
                <p>Detailed analysis of your content engagement</p>
                
                <div class="chart-container">
                    <canvas id="engagementChart"></canvas>
                </div>
                
                <div class="stats-grid">
                    <div class="stat-card">
                        <div class="stat-icon"><i class="fas fa-heart"></i></div>
                        <div class="stat-value" id="totalLikes">0</div>
                        <div class="stat-label">Total Likes</div>
                    </div>
                    
                    <div class="stat-card">
                        <div class="stat-icon"><i class="fas fa-comment"></i></div>
                        <div class="stat-value" id="totalComments">0</div>
                        <div class="stat-label">Total Comments</div>
                    </div>
                    
                    <div class="stat-card">
                        <div class="stat-icon"><i class="fas fa-share"></i></div>
                        <div class="stat-value" id="totalShares">0</div>
                        <div class="stat-label">Total Shares</div>
                    </div>
                    
                    <div class="stat-card">
                        <div class="stat-icon"><i class="fas fa-eye"></i></div>
                        <div class="stat-value" id="totalViews">0</div>
                        <div class="stat-label">Total Views</div>
                    </div>
                </div>
            </div>
            
            <div id="audience" class="tab-content">
                <h2>Audience Demographics</h2>
                <p>Information about your followers</p>
                
                <div class="chart-container">
                    <canvas id="demographicsChart"></canvas>
                </div>
                
                <h3>Follower Activity Heatmap</h3>
                <div class="heatmap-container">
                    <div class="heatmap-wrapper">
                        <div class="heatmap-days">
                            <div class="heatmap-day">Monday</div>
                            <div class="heatmap-day">Tuesday</div>
                            <div class="heatmap-day">Wednesday</div>
                            <div class="heatmap-day">Thursday</div>
                            <div class="heatmap-day">Friday</div>
                            <div class="heatmap-day">Saturday</div>
                            <div class="heatmap-day">Sunday</div>
                        </div>
                        <div>
                            <div class="heatmap-hours">
                                <!-- Hour labels will be inserted by JavaScript -->
                            </div>
                            <div class="heatmap" id="activityHeatmap">
                                <!-- Heatmap cells will be inserted by JavaScript -->
                            </div>
                        </div>
                    </div>
                </div>
                
                <h3>Top Locations</h3>
                <div class="chart-container">
                    <canvas id="locationsChart"></canvas>
                </div>
            </div>
            
            <div id="content" class="tab-content">
                <h2>Content Performance</h2>
                <p>Analysis of your individual posts</p>
                
                <div class="chart-container">
                    <canvas id="contentChart"></canvas>
                </div>
                
                <h3>Hashtag Performance</h3>
                <div class="hashtag-list" id="hashtagPerformance">
                    <!-- Hashtag items will be inserted by JavaScript -->
                </div>
                
                <h3>Best Performing Posts</h3>
                <div id="topPosts" class="stats-grid">
                    <!-- Top posts will be inserted by JavaScript -->
                </div>
            </div>
            
            <div id="growth" class="tab-content">
                <h2>Growth Analytics</h2>
                <p>Track your account growth over time</p>
                
                <div class="chart-container">
                    <canvas id="followersGrowthChart"></canvas>
                </div>
                
                <div class="stats-grid">
                    <div class="stat-card">
                        <div class="stat-icon"><i class="fas fa-chart-line"></i></div>
                        <div class="stat-value" id="weeklyGrowth">0</div>
                        <div class="stat-label">Weekly Growth</div>
                    </div>
                    
                    <div class="stat-card">
                        <div class="stat-icon"><i class="fas fa-chart-bar"></i></div>
                        <div class="stat-value" id="monthlyGrowth">0</div>
                        <div class="stat-label">Monthly Growth</div>
                    </div>
                    
                    <div class="stat-card">
                        <div class="stat-icon"><i class="fas fa-bullseye"></i></div>
                        <div class="stat-value" id="peakDay">N/A</div>
                        <div class="stat-label">Peak Engagement Day</div>
                    </div>
                    
                    <div class="stat-card">
                        <div class="stat-icon"><i class="fas fa-clock"></i></div>
                        <div class="stat-value" id="bestTime">N/A</div>
                        <div class="stat-label">Best Posting Time</div>
                    </div>
                </div>
            </div>
            
            <div id="competitors" class="tab-content">
                <h2>Competitor Comparison</h2>
                <p>Compare your performance with competitors</p>
                
                <div class="control-buttons">
                    <button class="control-btn" onclick="addCompetitor()"><i class="fas fa-plus"></i> Add Competitor</button>
                </div>
                
                <div class="competitor-grid" id="competitorGrid">
                    <!-- Competitor cards will be inserted by JavaScript -->
                </div>
                
                <h3>Performance Comparison</h3>
                
                <div class="chart-row">
                    <div class="chart-half">
                        <div class="chart-container">
                            <canvas id="competitorFollowersChart"></canvas>
                        </div>
                    </div>
                    <div class="chart-half">
                        <div class="chart-container">
                            <canvas id="competitorEngagementChart"></canvas>
                        </div>
                    </div>
                </div>
                
                <div class="chart-row">
                    <div class="chart-half">
                        <div class="chart-container">
                            <canvas id="competitorGrowthChart"></canvas>
                        </div>
                    </div>
                    <div class="chart-half">
                        <div class="chart-container">
                            <canvas id="competitorPostsChart"></canvas>
                        </div>
                    </div>
                </div>
                
                <div class="chart-container">
                    <canvas id="competitorRadarChart"></canvas>
                </div>
            </div>
        </div>
    </main>
    
    <!-- Raw Data Modal -->
    <div class="modal" id="rawDataModal">
        <div class="modal-content">
            <div class="modal-header">
                <h2>Raw Data</h2>
                <button class="close-modal" onclick="closeModal('rawDataModal')">&times;</button>
            </div>
            <pre id="rawDataContent"></pre>
        </div>
    </div>
    
    <footer>
        <div class="container">
            <div class="copyright">
                <p>&copy; 2023 Instagram Analytics. All rights reserved.</p>
            </div>
        </div>
    </footer>
    
    <script>
        // API Configuration
        const API_BASE_URL = 'http://localhost:5000';
        let chartInstances = {};
        let isLoggedIn = false;
        let currentUser = null;
        let currentData = null;
        
        // Check if user is logged in on page load
        window.onload = function() {
            checkLoginStatus();
        };
        
        // Check login status from localStorage
        function checkLoginStatus() {
            const savedUser = localStorage.getItem('instagramUser');
            const loginTime = localStorage.getItem('loginTime');
            const currentTime = new Date().getTime();
            
            // Check if user was logged in within the last 24 hours
            if (savedUser && loginTime && (currentTime - loginTime < 24 * 60 * 60 * 1000)) {
                currentUser = JSON.parse(savedUser);
                isLoggedIn = true;
                showAnalyticsDashboard();
            } else {
                // Clear expired login data
                localStorage.removeItem('instagramUser');
                localStorage.removeItem('loginTime');
                showLoginSection();
            }
        }
        
        // Show login section, hide analytics
        function showLoginSection() {
            document.getElementById('loginSection').classList.remove('hidden');
            document.getElementById('analyticsDashboard').classList.add('hidden');
            isLoggedIn = false;
        }
        
        // Show analytics dashboard, hide login
        function showAnalyticsDashboard() {
            document.getElementById('loginSection').classList.add('hidden');
            document.getElementById('analyticsDashboard').classList.remove('hidden');
            document.getElementById('loggedInUser').textContent = currentUser?.username || 'user';
            isLoggedIn = true;
            
            // Load data if we have a profile to analyze
            if (currentUser?.profileToAnalyze) {
                document.getElementById('profileUsername').value = currentUser.profileToAnalyze;
                analyzeProfile();
            }
        }
        
        // Login function
        function login() {
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;
            
            if (!username || !password) {
                alert('Please enter both username and password');
                return;
            }
            
            // Simulate login process
            currentUser = {
                username: username,
                profileToAnalyze: username // Default to analyzing own profile
            };
            
            // Save to localStorage
            localStorage.setItem('instagramUser', JSON.stringify(currentUser));
            localStorage.setItem('loginTime', new Date().getTime());
            
            isLoggedIn = true;
            showAnalyticsDashboard();
            
            // Simulate API call to fetch initial data
            simulateDataFetch();
        }
        
        // Login with Instagram
        function loginWithInstagram() {
            // Simulate Instagram OAuth flow
            alert('Redirecting to Instagram for authentication...');
            
            // Simulate successful login after a delay
            setTimeout(() => {
                currentUser = {
                    username: 'instagram_user',
                    profileToAnalyze: 'instagram_user'
                };
                
                // Save to localStorage
                localStorage.setItem('instagramUser', JSON.stringify(currentUser));
                localStorage.setItem('loginTime', new Date().getTime());
                
                isLoggedIn = true;
                showAnalyticsDashboard();
                
                // Simulate API call to fetch initial data
                simulateDataFetch();
            }, 1000);
        }
        
        // Logout function
        function logout() {
            if (confirm('Are you sure you want to log out?')) {
                // Clear user data
                localStorage.removeItem('instagramUser');
                localStorage.removeItem('loginTime');
                
                currentUser = null;
                isLoggedIn = false;
                
                showLoginSection();
            }
        }
        
        // Analyze profile function
        function analyzeProfile() {
            const profileUsername = document.getElementById('profileUsername').value;
            
            if (!profileUsername) {
                alert('Please enter a username to analyze');
                return;
            }
            
            // Update current user with profile to analyze
            if (currentUser) {
                currentUser.profileToAnalyze = profileUsername;
                localStorage.setItem('instagramUser', JSON.stringify(currentUser));
            }
            
            document.getElementById('loggedInUser').textContent = profileUsername;
            
            // Show loading state
            document.getElementById('fileInfo').textContent = `Analyzing profile: ${profileUsername}`;
            document.getElementById('dataStatus').className = 'data-status';
            document.getElementById('dataStatus').textContent = 'Fetching data...';
            
            // Simulate API call to fetch profile data
            setTimeout(() => {
                simulateDataFetch();
            }, 1500);
        }
        
        // Simulate fetching data from API
        function simulateDataFetch() {
            const username = document.getElementById('profileUsername').value || currentUser?.username || 'instagram_user';
            
            // Generate mock data
            currentData = generateMockData(username);
            
            // Update UI with data
            updateDashboard(currentData);
            
            // Update status
            document.getElementById('dataStatus').className = 'data-status status-success';
            document.getElementById('dataStatus').textContent = 'Data successfully loaded and analyzed';
            document.getElementById('fileInfo').textContent = `Analyzed profile: ${username} | Last updated: ${new Date().toLocaleString()}`;
        }
        
        // Fetch real data (simulated)
        function fetchRealData() {
            document.getElementById('dataStatus').className = 'data-status';
            document.getElementById('dataStatus').textContent = 'Refreshing data...';
            
            setTimeout(() => {
                simulateDataFetch();
            }, 1000);
        }
        
        // Switch between tabs
        function switchTab(tabName) {
            // Hide all tab contents
            const tabContents = document.getElementsByClassName('tab-content');
            for (let i = 0; i < tabContents.length; i++) {
                tabContents[i].classList.remove('active');
            }
            
            // Deactivate all tabs
            const tabs = document.getElementsByClassName('tab');
            for (let i = 0; i < tabs.length; i++) {
                tabs[i].classList.remove('active');
            }
            
            // Activate selected tab and content
            document.getElementById(tabName).classList.add('active');
            
            // Find and activate the clicked tab
            for (let i = 0; i < tabs.length; i++) {
                if (tabs[i].textContent.toLowerCase().includes(tabName.toLowerCase())) {
                    tabs[i].classList.add('active');
                    break;
                }
            }
            
            // If switching to competitors tab, ensure we have competitor data
            if (tabName === 'competitors' && currentData) {
                updateCompetitorTab(currentData);
            }
        }
        
        // Update dashboard with data
        function updateDashboard(data) {
            // Update overview stats
            document.getElementById('followersCount').textContent = data.followers.toLocaleString();
            document.getElementById('followingCount').textContent = data.following.toLocaleString();
            document.getElementById('postsCount').textContent = data.postsCount.toLocaleString();
            document.getElementById('avgLikes').textContent = data.avgLikes.toLocaleString();
            document.getElementById('avgComments').textContent = data.avgComments.toLocaleString();
            document.getElementById('engagementRate').textContent = data.engagementRate + '%';
            
            // Update engagement stats
            document.getElementById('totalLikes').textContent = data.totalLikes.toLocaleString();
            document.getElementById('totalComments').textContent = data.totalComments.toLocaleString();
            document.getElementById('totalShares').textContent = data.totalShares.toLocaleString();
            document.getElementById('totalViews').textContent = data.totalViews.toLocaleString();
            
            // Update growth stats
            document.getElementById('weeklyGrowth').textContent = '+' + data.weeklyGrowth.toLocaleString();
            document.getElementById('monthlyGrowth').textContent = '+' + data.monthlyGrowth.toLocaleString();
            document.getElementById('peakDay').textContent = data.peakDay;
            document.getElementById('bestTime').textContent = data.bestTime;
            
            // Create or update charts
            createCharts(data);
            
            // Update heatmap
            updateHeatmap(data.activityHeatmap);
            
            // Update hashtag performance
            updateHashtagPerformance(data.hashtags);
            
            // Update top posts
            updateTopPosts(data.topPosts);
            
            // Update competitor tab
            updateCompetitorTab(data);
        }
        
        // Create charts with data
        function createCharts(data) {
            const chartOptions = {
                responsive: true,
                maintainAspectRatio: false,
                plugins: {
                    legend: {
                        position: 'top',
                    }
                }
            };
            
            // Destroy existing charts to avoid duplicates
            for (let chartId in chartInstances) {
                if (chartInstances[chartId]) {
                    chartInstances[chartId].destroy();
                }
            }
            
            // Growth Chart (Line)
            const growthCtx = document.getElementById('growthChart').getContext('2d');
            chartInstances.growthChart = new Chart(growthCtx, {
                type: 'line',
                data: {
                    labels: data.followersGrowth.labels,
                    datasets: [{
                        label: 'Followers Growth',
                        data: data.followersGrowth.data,
                        borderColor: '#405DE6',
                        tension: 0.1,
                        fill: false
                    }]
                },
                options: chartOptions
            });
            
            // Engagement Chart (Bar)
            const engagementCtx = document.getElementById('engagementChart').getContext('2d');
            chartInstances.engagementChart = new Chart(engagementCtx, {
                type: 'bar',
                data: {
                    labels: data.engagementByDay.labels,
                    datasets: [{
                        label: 'Likes',
                        data: data.engagementByDay.likes,
                        backgroundColor: '#833AB4'
                    }, {
                        label: 'Comments',
                        data: data.engagementByDay.comments,
                        backgroundColor: '#E1306C'
                    }]
                },
                options: chartOptions
            });
            
            // Demographics Chart (Doughnut)
            const demographicsCtx = document.getElementById('demographicsChart').getContext('2d');
            chartInstances.demographicsChart = new Chart(demographicsCtx, {
                type: 'doughnut',
                data: {
                    labels: data.audienceDemographics.labels,
                    datasets: [{
                        data: data.audienceDemographics.data,
                        backgroundColor: [
                            '#405DE6', '#5851DB', '#833AB4', '#C13584', 
                            '#E1306C', '#FD1D1D', '#F56040', '#F77737'
                        ]
                    }]
                },
                options: chartOptions
            });
            
            // Locations Chart (Bar - Horizontal)
            const locationsCtx = document.getElementById('locationsChart').getContext('2d');
            chartInstances.locationsChart = new Chart(locationsCtx, {
                type: 'bar',
                data: {
                    labels: data.topLocations.labels,
                    datasets: [{
                        label: 'Followers',
                        data: data.topLocations.data,
                        backgroundColor: '#405DE6'
                    }]
                },
                options: {
                    indexAxis: 'y',
                    ...chartOptions
                }
            });
            
            // Content Chart (Line)
            const contentCtx = document.getElementById('contentChart').getContext('2d');
            chartInstances.contentChart = new Chart(contentCtx, {
                type: 'line',
                data: {
                    labels: data.contentPerformance.labels,
                    datasets: [{
                        label: 'Post Engagement',
                        data: data.contentPerformance.data,
                        borderColor: '#E1306C',
                        tension: 0.1,
                        fill: false
                    }]
                },
                options: chartOptions
            });
            
            // Followers Growth Chart (Line)
            const followersGrowthCtx = document.getElementById('followersGrowthChart').getContext('2d');
            chartInstances.followersGrowthChart = new Chart(followersGrowthCtx, {
                type: 'line',
                data: {
                    labels: data.followersGrowth.labels,
                    datasets: [{
                        label: 'Followers',
                        data: data.followersGrowth.data,
                        borderColor: '#405DE6',
                        tension: 0.1,
                        fill: true,
                        backgroundColor: 'rgba(64, 93, 230, 0.1)'
                    }]
                },
                options: chartOptions
            });
            
            // Competitor Followers Chart (Bar)
            const competitorFollowersCtx = document.getElementById('competitorFollowersChart').getContext('2d');
            chartInstances.competitorFollowersChart = new Chart(competitorFollowersCtx, {
                type: 'bar',
                data: {
                    labels: ['Your Account', 'Competitor 1', 'Competitor 2', 'Competitor 3', 'Competitor 4', 'Competitor 5'],
                    datasets: [{
                        label: 'Followers',
                        data: [
                            data.followers,
                            Math.round(data.followers * 0.8),
                            Math.round(data.followers * 1.2),
                            Math.round(data.followers * 0.6),
                            Math.round(data.followers * 1.5),
                            Math.round(data.followers * 0.9)
                        ],
                        backgroundColor: '#405DE6'
                    }]
                },
                options: chartOptions
            });
            
            // Competitor Engagement Chart (Bar)
            const competitorEngagementCtx = document.getElementById('competitorEngagementChart').getContext('2d');
            chartInstances.competitorEngagementChart = new Chart(competitorEngagementCtx, {
                type: 'bar',
                data: {
                    labels: ['Your Account', 'Competitor 1', 'Competitor 2', 'Competitor 3', 'Competitor 4', 'Competitor 5'],
                    datasets: [{
                        label: 'Engagement Rate (%)',
                        data: [
                            data.engagementRate,
                            (data.engagementRate * 0.7).toFixed(2),
                            (data.engagementRate * 1.1).toFixed(2),
                            (data.engagementRate * 0.8).toFixed(2),
                            (data.engagementRate * 1.3).toFixed(2),
                            (data.engagementRate * 0.9).toFixed(2)
                        ],
                        backgroundColor: '#E1306C'
                    }]
                },
                options: chartOptions
            });
            
            // Competitor Growth Chart (Line)
            const competitorGrowthCtx = document.getElementById('competitorGrowthChart').getContext('2d');
            chartInstances.competitorGrowthChart = new Chart(competitorGrowthCtx, {
                type: 'line',
                data: {
                    labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
                    datasets: [
                        {
                            label: 'Your Account',
                            data: [data.monthlyGrowth, data.monthlyGrowth + 50, data.monthlyGrowth + 120, data.monthlyGrowth + 200, data.monthlyGrowth + 280, data.monthlyGrowth + 350],
                            borderColor: '#405DE6',
                            tension: 0.1
                        },
                        {
                            label: 'Competitor Avg',
                            data: [data.monthlyGrowth * 0.7, data.monthlyGrowth * 0.7 + 40, data.monthlyGrowth * 0.7 + 90, data.monthlyGrowth * 0.7 + 150, data.monthlyGrowth * 0.7 + 220, data.monthlyGrowth * 0.7 + 300],
                            borderColor: '#833AB4',
                            tension: 0.1
                        }
                    ]
                },
                options: chartOptions
            });
            
            // Competitor Posts Chart (Doughnut)
            const competitorPostsCtx = document.getElementById('competitorPostsChart').getContext('2d');
            chartInstances.competitorPostsChart = new Chart(competitorPostsCtx, {
                type: 'doughnut',
                data: {
                    labels: ['Your Account', 'Competitor 1', 'Competitor 2', 'Competitor 3', 'Competitor 4', 'Competitor 5'],
                    datasets: [{
                        data: [
                            data.postsCount,
                            Math.round(data.postsCount * 0.8),
                            Math.round(data.postsCount * 1.2),
                            Math.round(data.postsCount * 0.6),
                            Math.round(data.postsCount * 1.5),
                            Math.round(data.postsCount * 0.9)
                        ],
                        backgroundColor: [
                            '#405DE6', '#5851DB', '#833AB4', '#C13584', '#E1306C', '#FD1D1D'
                        ]
                    }]
                },
                options: chartOptions
            });
            
            // Competitor Radar Chart
            const competitorRadarCtx = document.getElementById('competitorRadarChart').getContext('2d');
            chartInstances.competitorRadarChart = new Chart(competitorRadarCtx, {
                type: 'radar',
                data: {
                    labels: ['Followers', 'Engagement', 'Growth', 'Consistency', 'Content Quality', 'Post Frequency'],
                    datasets: [
                        {
                            label: 'Your Account',
                            data: [85, 75, 80, 90, 70, 65],
                            backgroundColor: 'rgba(64, 93, 230, 0.2)',
                            borderColor: '#405DE6',
                            pointBackgroundColor: '#405DE6'
                        },
                        {
                            label: 'Top Competitor',
                            data: [95, 65, 70, 80, 85, 75],
                            backgroundColor: 'rgba(131, 58, 180, 0.2)',
                            borderColor: '#833AB4',
                            pointBackgroundColor: '#833AB4'
                        }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        title: {
                            display: true,
                            text: 'Performance Comparison Radar'
                        }
                    }
                }
            });
        }
        
        // Update heatmap with data
        function updateHeatmap(heatmapData) {
            const heatmapContainer = document.getElementById('activityHeatmap');
            heatmapContainer.innerHTML = '';
            
            const hoursContainer = document.querySelector('.heatmap-hours');
            hoursContainer.innerHTML = '';
            
            // Create hour labels
            for (let i = 0; i < 24; i++) {
                const hourLabel = document.createElement('div');
                hourLabel.className = 'heatmap-hour-label';
                hourLabel.textContent = `${i}:00`;
                hoursContainer.appendChild(hourLabel);
            }
            
            // Create heatmap cells
            for (let day = 0; day < 7; day++) {
                for (let hour = 0; hour < 24; hour++) {
                    const value = heatmapData[day][hour];
                    const cell = document.createElement('div');
                    cell.className = 'heatmap-hour';
                    
                    // Determine intensity class based on value
                    if (value >= 80) cell.classList.add('extreme');
                    else if (value >= 60) cell.classList.add('very-high');
                    else if (value >= 40) cell.classList.add('high');
                    else if (value >= 20) cell.classList.add('medium');
                    else cell.classList.add('low');
                    
                    // Add tooltip
                    const dayNames = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'];
                    cell.setAttribute('data-tooltip', `${dayNames[day]} ${hour}:00 - Activity: ${value}%`);
                    
                    heatmapContainer.appendChild(cell);
                }
            }
        }
        
        // Update hashtag performance
        function updateHashtagPerformance(hashtags) {
            const container = document.getElementById('hashtagPerformance');
            container.innerHTML = '';
            
            hashtags.forEach(hashtag => {
                const item = document.createElement('div');
                item.className = 'hashtag-item';
                
                item.innerHTML = `
                    <div class="hashtag-name">#${hashtag.name}</div>
                    <div class="hashtag-stats">
                        <div class="hashtag-stat">
                            <div class="hashtag-stat-value">${hashtag.posts}</div>
                            <div class="hashtag-stat-label">Posts</div>
                        </div>
                        <div class="hashtag-stat">
                            <div class="hashtag-stat-value">${hashtag.engagement}</div>
                            <div class="hashtag-stat-label">Engagement</div>
                        </div>
                        <div class="hashtag-stat">
                            <div class="hashtag-stat-value">${hashtag.reach}</div>
                            <div class="hashtag-stat-label">Reach</div>
                        </div>
                    </div>
                `;
                
                container.appendChild(item);
            });
        }
        
        // Update top posts
        function updateTopPosts(posts) {
            const container = document.getElementById('topPosts');
            container.innerHTML = '';
            
            posts.forEach(post => {
                const card = document.createElement('div');
                card.className = 'stat-card';
                
                card.innerHTML = `
                    <div class="stat-icon"><i class="fas fa-heart"></i></div>
                    <div class="stat-value">${post.likes.toLocaleString()}</div>
                    <div class="stat-label">Likes</div>
                    <div style="margin-top: 10px; font-size: 12px; color: #8e8e8e;">${post.date}</div>
                `;
                
                container.appendChild(card);
            });
        }
        
        // Update competitor tab
        function updateCompetitorTab(data) {
            const container = document.getElementById('competitorGrid');
            container.innerHTML = '';
            
            // Add your account as the first card
            addCompetitorCard(container, {
                name: data.username,
                followers: data.followers,
                engagement: data.engagementRate,
                posts: data.postsCount,
                avgLikes: data.avgLikes,
                avatar: data.username.charAt(0).toUpperCase()
            }, true);
            
            // Add 5 competitor accounts
            for (let i = 1; i <= 5; i++) {
                const followerDiff = i % 2 === 0 ? 1.2 : 0.8;
                const engagementDiff = i % 2 === 0 ? 0.9 : 1.1;
                
                addCompetitorCard(container, {
                    name: `competitor_${i}`,
                    followers: Math.round(data.followers * followerDiff),
                    engagement: (data.engagementRate * engagementDiff).toFixed(2),
                    posts: Math.round(data.postsCount * (0.8 + i * 0.1)),
                    avgLikes: Math.round(data.avgLikes * (0.9 + i * 0.05)),
                    avatar: `C${i}`
                }, false);
            }
        }
        
        // Add a competitor card to the grid
        function addCompetitorCard(container, competitor, isYourAccount) {
            const card = document.createElement('div');
            card.className = 'competitor-card';
            
            card.innerHTML = `
                <div class="competitor-header">
                    <div class="competitor-avatar">${competitor.avatar}</div>
                    <div class="competitor-name">${competitor.name} ${isYourAccount ? '(Your Account)' : ''}</div>
                </div>
                <div class="competitor-stats">
                    <div class="competitor-stat">
                        <div class="competitor-stat-value">${competitor.followers.toLocaleString()}</div>
                        <div class="competitor-stat-label">Followers</div>
                    </div>
                    <div class="competitor-stat">
                        <div class="competitor-stat-value">${competitor.engagement}%</div>
                        <div class="competitor-stat-label">Engagement</div>
                    </div>
                    <div class="competitor-stat">
                        <div class="competitor-stat-value">${competitor.posts.toLocaleString()}</div>
                        <div class="competitor-stat-label">Posts</div>
                    </div>
                    <div class="competitor-stat">
                        <div class="competitor-stat-value">${competitor.avgLikes.toLocaleString()}</div>
                        <div class="competitor-stat-label">Avg. Likes</div>
                    </div>
                </div>
            `;
            
            container.appendChild(card);
        }
        
        // Add a competitor (simulated)
        function addCompetitor() {
            const username = prompt('Enter competitor Instagram username:');
            if (username) {
                // Simulate adding a competitor
                alert(`Added ${username} as a competitor. Data will be loaded on next refresh.`);
                fetchRealData();
            }
        }
        
        // Export to PDF
        function exportToPDF() {
            alert('Exporting to PDF... This would generate a PDF report in a real application.');
        }
        
        // View raw data
        function viewRawData() {
            if (currentData) {
                document.getElementById('rawDataContent').textContent = JSON.stringify(currentData, null, 2);
                openModal('rawDataModal');
            } else {
                alert('No data available to display. Please analyze a profile first.');
            }
        }
        
        // Open modal
        function openModal(modalId) {
            document.getElementById(modalId).style.display = 'flex';
        }
        
        // Close modal
        function closeModal(modalId) {
            document.getElementById(modalId).style.display = 'none';
        }
        
        // Generate mock data for demonstration
        function generateMockData(username) {
            // Generate random data for demonstration
            const followers = Math.floor(Math.random() * 100000) + 5000;
            const following = Math.floor(Math.random() * 2000) + 100;
            const postsCount = Math.floor(Math.random() * 500) + 50;
            const avgLikes = Math.floor(followers * (Math.random() * 0.05 + 0.03));
            const avgComments = Math.floor(avgLikes * (Math.random() * 0.1 + 0.05));
            const engagementRate = ((avgLikes + avgComments) / followers * 100).toFixed(2);
            
            // Generate dates for the last 30 days
            const dates = [];
            for (let i = 29; i >= 0; i--) {
                const date = new Date();
                date.setDate(date.getDate() - i);
                dates.push(date.toLocaleDateString('en-US', { month: 'short', day: 'numeric' }));
            }
            
            // Generate followers growth data
            const followersGrowthData = [];
            let currentFollowers = followers - Math.floor(Math.random() * 1000);
            for (let i = 0; i < 30; i++) {
                currentFollowers += Math.floor(Math.random() * 50);
                followersGrowthData.push(currentFollowers);
            }
            
            // Generate engagement by day data
            const engagementByDayData = {
                labels: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
                likes: [],
                comments: []
            };
            
            for (let i = 0; i < 7; i++) {
                engagementByDayData.likes.push(Math.floor(Math.random() * 500) + 200);
                engagementByDayData.comments.push(Math.floor(Math.random() * 50) + 20);
            }
            
            // Generate audience demographics data
            const audienceDemographicsData = {
                labels: ['18-24', '25-34', '35-44', '45-54', '55+'],
                data: [
                    Math.floor(Math.random() * 30) + 40,
                    Math.floor(Math.random() * 20) + 25,
                    Math.floor(Math.random() * 15) + 15,
                    Math.floor(Math.random() * 10) + 5,
                    Math.floor(Math.random() * 5) + 2
                ]
            };
            
            // Generate top locations data
            const topLocationsData = {
                labels: ['United States', 'United Kingdom', 'India', 'Canada', 'Australia', 'Germany', 'Brazil'],
                data: [
                    Math.floor(Math.random() * 30) + 25,
                    Math.floor(Math.random() * 15) + 10,
                    Math.floor(Math.random() * 20) + 15,
                    Math.floor(Math.random() * 10) + 5,
                    Math.floor(Math.random() * 10) + 5,
                    Math.floor(Math.random() * 8) + 5,
                    Math.floor(Math.random() * 12) + 8
                ]
            };
            
            // Generate content performance data
            const contentPerformanceData = {
                labels: dates,
                data: []
            };
            
            for (let i = 0; i < 30; i++) {
                contentPerformanceData.data.push(Math.floor(Math.random() * 100) + 50);
            }
            
            // Generate activity heatmap data
            const activityHeatmapData = [];
            for (let day = 0; day < 7; day++) {
                const dayData = [];
                for (let hour = 0; hour < 24; hour++) {
                    // Create a pattern with higher activity during daytime
                    let baseActivity = 0;
                    if (hour >= 8 && hour <= 12) baseActivity = 40;
                    else if (hour >= 13 && hour <= 17) baseActivity = 60;
                    else if (hour >= 18 && hour <= 22) baseActivity = 80;
                    else baseActivity = 20;
                    
                    // Add some randomness
                    const activity = Math.min(100, Math.max(0, baseActivity + (Math.random() * 30 - 15)));
                    dayData.push(Math.round(activity));
                }
                activityHeatmapData.push(dayData);
            }
            
            // Generate hashtag performance data
            const hashtagsData = [];
            const popularHashtags = ['love', 'instagood', 'photooftheday', 'fashion', 'beautiful', 'happy', 'follow', 'like', 'photo', 'art'];
            
            for (let i = 0; i < 5; i++) {
                hashtagsData.push({
                    name: popularHashtags[i],
                    posts: Math.floor(Math.random() * 100) + 50,
                    engagement: Math.floor(Math.random() * 1000) + 500,
                    reach: Math.floor(Math.random() * 10000) + 5000
                });
            }
            
            // Generate top posts data
            const topPostsData = [];
            for (let i = 0; i < 4; i++) {
                const date = new Date();
                date.setDate(date.getDate() - Math.floor(Math.random() * 30));
                
                topPostsData.push({
                    likes: Math.floor(Math.random() * 5000) + 1000,
                    comments: Math.floor(Math.random() * 500) + 100,
                    date: date.toLocaleDateString()
                });
            }
            
            // Return the complete mock data object
            return {
                username: username,
                followers: followers,
                following: following,
                postsCount: postsCount,
                avgLikes: avgLikes,
                avgComments: avgComments,
                engagementRate: engagementRate,
                totalLikes: avgLikes * postsCount,
                totalComments: avgComments * postsCount,
                totalShares: Math.floor(avgLikes * postsCount * 0.2),
                totalViews: Math.floor(avgLikes * postsCount * 3),
                weeklyGrowth: Math.floor(Math.random() * 500) + 100,
                monthlyGrowth: Math.floor(Math.random() * 2000) + 500,
                peakDay: ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'][Math.floor(Math.random() * 7)],
                bestTime: `${Math.floor(Math.random() * 12) + 1}:00 ${Math.random() > 0.5 ? 'PM' : 'AM'}`,
                followersGrowth: {
                    labels: dates,
                    data: followersGrowthData
                },
                engagementByDay: engagementByDayData,
                audienceDemographics: audienceDemographicsData,
                topLocations: topLocationsData,
                contentPerformance: contentPerformanceData,
                activityHeatmap: activityHeatmapData,
                hashtags: hashtagsData,
                topPosts: topPostsData
            };
        }
        
        // Set up event listeners
        document.getElementById('logoutBtn').addEventListener('click', logout);
        
        // Close modal when clicking outside
        window.addEventListener('click', function(event) {
            const modals = document.getElementsByClassName('modal');
            for (let i = 0; i < modals.length; i++) {
                if (event.target === modals[i]) {
                    modals[i].style.display = 'none';
                }
            }
        });
    </script>
</body>
</html>
