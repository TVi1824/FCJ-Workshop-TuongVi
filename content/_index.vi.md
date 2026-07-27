---
title: "Báo cáo thực tập"
date: 2024-01-01
weight: 1
chapter: false
---

<style>
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

  .custom-profile-container {
    font-family: 'Inter', sans-serif;
    width: 100%;
    max-width: 1100px;
    margin: 0 auto;
    color: #333;
  }

  .header-section {
    display: flex;
    align-items: center;
    gap: 40px;
    margin-bottom: 0px;
    position: relative;
  }

  .avatar-card {
    background: transparent;
    flex-shrink: 0;
    width: 260px;
  }

  .avatar-card img {
    width: 100%;
    height: auto;
    display: block;
    margin: 0;
  }

  .info-content {
    flex-grow: 1;
    position: relative;
  }

  .info-content .main-title {
    display: inline-block;
    font-size: 2.5rem;
    font-weight: 700;
    margin: 0 0 15px 0;
    padding: 0 0 15px 0;
    text-align: left;
    text-transform: uppercase;
    position: relative;
    border: none;
    line-height: 1.2;
    color: #718096;
  }
  
  .info-content .main-title::after {
    content: '';
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    bottom: 0;
    width: 120px;
    height: 3px;
    background: linear-gradient(90deg, transparent, #3182ce, transparent);
    border-radius: 2px;
  }

  .info-content .name {
    font-size: 1.6rem;
    font-weight: 500;
    margin: 20px 0 8px 0;
    color: #2d3748;
  }

  .info-content .desc {
    font-size: 1.05rem;
    color: #718096;
    margin: 0;
  }

  .cards-section {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 15px;
    width: 100%;
  }

  .info-card {
    background: #fff;
    border-radius: 12px;
    padding: 16px;
    box-shadow: 0 8px 24px rgba(0,0,0,0.06);
    display: flex;
    flex-direction: column;
  }

  .info-card h3 {
    font-size: 1.3rem;
    font-weight: 600;
    margin-top: 0;
    margin-bottom: 15px;
    color: #2d3748;
  }

  .info-card p {
    font-size: 0.9rem;
    line-height: 1.6;
    margin-top: 0;
    margin-bottom: 10px;
    color: #4a5568;
    word-break: normal;
    overflow-wrap: break-word;
  }

  .info-card p:last-child {
    margin-bottom: 0;
  }
  
  .info-card strong {
    font-weight: 600;
    color: #1a202c;
  }

  /* Responsive */
  @media (max-width: 850px) {
    .header-section {
      flex-direction: column;
      text-align: center;
    }
    
    .avatar-card {
      margin: 0 auto;
    }

    .cards-section {
      grid-template-columns: 1fr;
    }
  }
</style>

<div class="custom-profile-container">
<div class="header-section">
<div class="avatar-card">
<img src="/images/avatar.png" alt="Nguyễn Trịnh Tường Vi">
</div>
<div class="info-content">
<div class="main-title">BÁO CÁO THỰC TẬP</div>
<p class="name">Nguyễn Trịnh Tường Vi</p>
<p class="desc">Workforce Bootcamp - First Cloud AI Journey</p>
</div>
</div>
<div class="cards-section">
<div class="info-card">
<h3>Liên hệ</h3>
<p><strong>Điện thoại:</strong> 0855900997</p>
<p><strong>Email:</strong><br>nguyenvii1824@gmail.com</p>
</div>
<div class="info-card">
<h3>Học vấn</h3>
<p><strong>Trường:</strong> Trường Đại học Công Nghệ TP.HCM</p>
<p><strong>Ngành:</strong> Công nghệ thông tin</p>
<p><strong>Lớp:</strong> 22DTHH5</p>
</div>
<div class="info-card">
<h3>Công ty</h3>
<p><strong>Công ty thực tập:</strong> Công ty TNHH Amazon Web Services Việt Nam</p>
<p><strong>Thời gian thực tập:</strong> Từ 05/05/2026 đến 30/07/2026</p>
</div>
</div>
</div>



### Nội dung báo cáo

1.  [Worklog](1-Worklog/)
2.  [Proposal](2-Proposal/)
3.  [Các bài blogs đã đăng](3-BlogsPosted/)
4.  [Các events đã tham gia](4-EventParticipated/)
5.  [Workshop](5-Workshop/)
6.  [Tự đánh giá](6-Self-evaluation/)
7.  [Chia sẻ, đóng góp ý kiến](7-Feedback/)
8. [Resources](8-Resources/)

<style>
  img {
    max-width: 100%;
    height: auto;
  }
  .bouncing-cat {
    position: fixed;
    bottom: 20px;
    right: 20px;
    z-index: 99999;
    animation: boing 0.4s infinite alternate ease-in-out;
  }
  .bouncing-cat img {
    border: none !important;
    box-shadow: none !important;
    background: transparent !important;
    padding: 0 !important;
    margin: 0 !important;
  }
  @keyframes boing {
    0% { transform: translateY(0); }
    100% { transform: translateY(-40px); }
  }
</style>
<div class="bouncing-cat">
  <img src="../cat.gif" alt="Bouncing Cat" width="120">
</div>
