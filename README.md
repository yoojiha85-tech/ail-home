<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>아일항공여행사 - 아일항공과 함께하는 완벽한 허니문</title>
    
    <!-- External Assets -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;500;700;900&family=Playfair+Display:ital,wght@0,400;0,700;1,400&display=swap" rel="stylesheet">

    <style>
        :root {
            --primary-rose: #e88d8d;
            --primary-blue: #0052cc;
            --secondary-gold: #d4a373;
            --naver-green: #2DB400;
            --bg-soft: #fcf9f7;
            --text-main: #2d3436;
        }

        body {
            font-family: 'Noto Sans KR', sans-serif;
            background-color: var(--bg-soft);
            color: var(--text-main);
            scroll-behavior: smooth;
            -webkit-tap-highlight-color: transparent;
            overflow-x: hidden;
        }

        .serif-font { font-family: 'Playfair Display', serif; }

        /* Navigation Optimization */
        nav {
            background: rgba(255, 255, 255, 0.98);
            backdrop-filter: blur(15px);
            z-index: 2000;
            transition: all 0.3s ease;
        }

        /* Hero Map Section */
        #map {
            height: 75vh;
            width: 100%;
            background: #d1d8e0;
        }
        .area-label {
            background: rgba(255, 255, 255, 0.95);
            border: 1px solid #e1e1e1;
            font-weight: 700;
            padding: 3px 8px;
            border-radius: 6px;
            font-size: 11px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
        }
        .marker-dot {
            width: 12px; height: 12px;
            border-radius: 50%;
            background: #ff4757;
            border: 2.5px solid white;
            box-shadow: 0 0 10px rgba(255, 71, 87, 0.4);
        }
        .icn-dot {
            width: 16px; height: 16px;
            background: #1e90ff;
            animation: pulse 2s infinite;
        }
        @keyframes pulse {
            0% { transform: scale(1); box-shadow: 0 0 0 0 rgba(30, 144, 255, 0.7); }
            70% { transform: scale(1.3); box-shadow: 0 0 0 12px rgba(30, 144, 255, 0); }
            100% { transform: scale(1); box-shadow: 0 0 0 0 rgba(30, 144, 255, 0); }
        }

        /* Section Title Redesign */
        .section-header {
            text-align: center;
            margin-bottom: 4rem;
        }
        .section-header i {
            display: block;
            font-size: 2rem;
            color: var(--secondary-gold);
            margin-bottom: 1rem;
            opacity: 0.8;
        }
        .section-header h2 {
            font-size: 2.25rem;
            font-weight: 900;
            letter-spacing: -0.05em;
            position: relative;
            display: inline-block;
        }
        .section-header h2::after {
            content: '';
            position: absolute;
            bottom: -15px;
            left: 50%;
            transform: translateX(-50%);
            width: 40px;
            height: 3px;
            background: var(--secondary-gold);
            border-radius: 2px;
        }

        /* Gift Cards */
        .gift-card { 
            transition: all 0.4s cubic-bezier(0.165, 0.84, 0.44, 1); 
            cursor: pointer; 
            border: 1px solid #f1f1f1;
            background: white;
        }
        .gift-card:hover { transform: translateY(-8px); box-shadow: 0 20px 40px rgba(0,0,0,0.06); }
        .gift-card.selected { border: 2px solid var(--primary-rose); background-color: #fffafc; }
        
        .img-grid { display: grid; gap: 0.75rem; margin: 1.5rem 0; }
        .grid-4 { grid-template-columns: repeat(4, 1fr); }
        .grid-3 { grid-template-columns: repeat(3, 1fr); }
        
        .img-item { 
            aspect-ratio: 1/1; 
            object-fit: cover; 
            border-radius: 0.75rem; 
            transition: transform 0.3s ease; 
            cursor: zoom-in; 
            background: #f8f8f8; 
            width: 100%; 
            border: 1px solid #f1f1f1; 
        }
        .img-item:hover { transform: scale(1.08); z-index: 10; }
        .img-caption { font-size: 0.75rem; color: #95a5a6; margin-top: 6px; text-align: center; font-weight: 500; }

        /* Timeline Box */
        .timeline-box {
            position: relative;
            padding-left: 3rem;
            border-left: 2px dashed #dfe6e9;
            margin-left: 1.5rem;
            padding-bottom: 3rem;
        }
        .timeline-box:last-child { border-left-color: transparent; }
        .timeline-box::before {
            content: '';
            position: absolute;
            left: -11px;
            top: 0;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: white;
            border: 4px solid var(--secondary-gold);
            box-shadow: 0 0 10px rgba(212, 163, 115, 0.3);
        }

        /* Animation Classes */
        .fade-in { opacity: 0; transform: translateY(30px); transition: all 0.8s ease-out; }
        .fade-in.visible { opacity: 1; transform: translateY(0); }

        @media (max-width: 768px) {
            .section-header h2 { font-size: 1.75rem; }
            #map { height: 60vh; }
            .timeline-box { padding-left: 2rem; }
        }
    </style>
</head>
<body>

    <!-- Header & Navigation -->
    <nav class="fixed top-0 left-0 w-full border-b border-gray-100 px-6 py-3 flex justify-between items-center shadow-sm">
        <div class="flex items-center gap-3">
            <img src="https://postfiles.pstatic.net/MjAyNjAxMjZfMTU0/MDAxNzY5NDA0MTc2MjI4.qeqS5GSxL2dxnwEkAIJsMJyxHZcOvLYxlZph5tlVIRUg.6-iuP5mhmHKmpr_PnQ2IHVH0vILZsjQEBDYylCoUVNsg.PNG/bg.png?type=w580" 
                 alt="아일항공여행사 로고" 
                 class="h-10 md:h-12 w-auto object-contain">
            <div class="text-xl md:text-2xl font-black tracking-tighter text-gray-900 leading-none">
                아일항공여행사
            </div>
        </div>
        <div class="hidden xl:flex space-x-8 text-[15px] font-bold text-gray-600">
            <a href="#map-section" class="hover:text-amber-700 transition-colors">허니문 지도</a>
            <a href="#guide-section" class="hover:text-amber-700 transition-colors">예약 안내</a>
            <a href="#pre-section" class="hover:text-amber-700 transition-colors">사전 예약</a>
            <a href="#gift-section" class="hover:text-amber-700 transition-colors">스페셜 사은품</a>
            <a href="#event-section" class="hover:text-amber-700 transition-colors">지인 추천</a>
            <a href="#review-section" class="hover:text-amber-700 transition-colors">여행 후기</a>
        </div>
        <a href="https://map.naver.com/p/search/%EC%95%84%EC%9D%BC%ED%95%AD%EA%B3%B5%EC%97%AC%ED%96%89%EC%82%AC/place/1983566624" target="_blank" class="bg-amber-700 text-white px-6 py-2.5 rounded-full text-sm font-black shadow-lg shadow-amber-100 hover:bg-amber-800 transition-all">
            무료 상담 예약
        </a>
    </nav>

    <div class="pt-16">
        <!-- Section 1: Honeymoon World Map (31개 도시 전체 복원) -->
        <section id="map-section" class="relative">
            <div class="absolute top-8 left-1/2 -translate-x-1/2 z-[1001] bg-white/95 px-8 py-4 rounded-[2rem] shadow-2xl border border-white/50 text-center min-w-[320px]">
                <h2 class="text-lg md:text-xl font-black text-gray-800 flex items-center justify-center gap-2">
                    🌏 Honeymoon World Map
                </h2>
                <p class="text-xs text-amber-700 font-bold mt-1">인천공항(ICN) 출발 · 전 세계 허니문 노선도</p>
            </div>
            <div id="map"></div>
        </section>

        <!-- Section 2: Guide (8단계 가이드 완벽 유지) -->
        <section id="guide-section" class="py-24 bg-white">
            <div class="container mx-auto px-6 max-w-6xl">
                <div class="section-header">
                    <i class="fas fa-map-signs"></i>
                    <h2>허니문 예약 진행 안내</h2>
                    <p class="text-gray-500 mt-4 font-medium">당신의 인생에서 가장 소중한 여행, 아일항공이 꼼꼼하게 준비합니다.</p>
                </div>
                
                <div class="max-w-3xl mx-auto mt-16">
                    <div class="timeline-box fade-in">
                        <span class="text-xs font-black text-amber-600 uppercase tracking-widest mb-1 block">Phase 01</span>
                        <h3 class="font-bold text-xl mb-2 text-gray-900">신혼여행 지역 선택</h3>
                        <p class="text-gray-500 text-[15px] leading-relaxed">고객님의 취향과 여행 스타일에 맞는 최적의 허니문 지역을 전문가 상담을 통해 결정합니다.</p>
                    </div>
                    <div class="timeline-box fade-in">
                        <span class="text-xs font-black text-amber-600 uppercase tracking-widest mb-1 block">Phase 02</span>
                        <h3 class="font-bold text-xl mb-2 text-gray-900">출발일 및 한국 도착일 결정</h3>
                        <p class="text-gray-500 text-[15px] leading-relaxed">예식 일정과 휴가 계획을 고려하여 가장 여유로운 출발 및 귀국 일정을 확정합니다.</p>
                    </div>
                    <div class="timeline-box fade-in">
                        <span class="text-xs font-black text-amber-600 uppercase tracking-widest mb-1 block">Phase 03</span>
                        <h3 class="font-bold text-xl mb-2 text-gray-900">항공사별 스케줄 및 요금 확인</h3>
                        <p class="text-gray-500 text-[15px] leading-relaxed">원하시는 일정에 맞춰 이용 가능한 모든 항공편의 스케줄과 합리적인 요금을 안내해 드립니다.</p>
                    </div>
                    <div class="timeline-box fade-in bg-blue-50/50 p-6 rounded-2xl border border-blue-100 mb-8 ml-6">
                        <span class="inline-block px-3 py-1 bg-blue-600 text-white text-[10px] font-black rounded-full mb-3">BEST SERVICE</span>
                        <h3 class="font-bold text-xl mb-2 text-blue-800">항공 예약 [무료 홀딩 / 좌석 확보]</h3>
                        <p class="text-gray-700 text-[15px] leading-relaxed">여권상의 영문 성함만으로 <strong>72시간 동안</strong> 현재 요금과 좌석을 무료로 홀딩하여 고민하실 시간을 드립니다.</p>
                    </div>
                    <div class="timeline-box fade-in">
                        <span class="text-xs font-black text-amber-600 uppercase tracking-widest mb-1 block">Phase 05</span>
                        <h3 class="font-bold text-xl mb-2 text-gray-900">항공권 결정 및 발권(결제)</h3>
                        <p class="text-gray-500 text-[15px] leading-relaxed">최종 항공편이 확정되면 발권을 진행하며, 이 시점에 항공권 결제가 안전하게 이루어집니다.</p>
                    </div>
                    <div class="timeline-box fade-in">
                        <span class="text-xs font-black text-amber-600 uppercase tracking-widest mb-1 block">Phase 06</span>
                        <h3 class="font-bold text-xl mb-2 text-gray-900">일정 및 호텔/객실 타입 선택</h3>
                        <p class="text-gray-500 text-[15px] leading-relaxed">여행지에서의 상세 일정과 숙소 타입(풀빌라, 오션뷰 등)을 결정하고 최종 예약 및 계약서를 작성합니다.</p>
                    </div>
                    <div class="timeline-box fade-in">
                        <span class="text-xs font-black text-amber-600 uppercase tracking-widest mb-1 block">Phase 07</span>
                        <h3 class="font-bold text-xl mb-2 text-gray-900">출발 45일 전 파이널 체크</h3>
                        <p class="text-gray-500 text-[15px] leading-relaxed">전체 예약 내용을 다시 한번 꼼꼼히 확인하고, 잔금 결제 안내 및 여행 준비 사항을 공유합니다.</p>
                    </div>
                    <div class="timeline-box fade-in border-none">
                        <span class="text-xs font-black text-rose-500 uppercase tracking-widest mb-1 block">Final Phase</span>
                        <h3 class="font-bold text-xl mb-2 text-rose-600">사전 설명회 및 최종 일정 안내</h3>
                        <p class="text-gray-500 text-[15px] leading-relaxed">출발 7~15일 전, 확정서와 바우처를 전달드리며 현지 미팅 및 주의사항을 안내하는 설명회를 진행합니다.</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- Section 3: Pre-order Promotion -->
        <section id="pre-section" class="py-24 bg-slate-900 text-white">
            <div class="container mx-auto px-6 max-w-6xl">
                <div class="section-header">
                    <i class="fas fa-calendar-star text-amber-400"></i>
                    <h2 class="text-white">빠를수록 커지는 사전예약 할인</h2>
                    <p class="text-slate-400 mt-4 font-medium italic">"Early Birds Get the Best Deal"</p>
                </div>
                
                <div class="grid grid-cols-2 lg:grid-cols-4 gap-6 max-w-5xl mx-auto">
                    <div class="bg-white/5 p-8 rounded-3xl text-center border border-white/10 fade-in hover:bg-white/10 transition-all">
                        <p class="text-amber-400 font-black text-sm mb-4">출발 150일 전</p>
                        <h4 class="text-3xl font-black mb-2">100,000원</h4>
                        <p class="text-[11px] text-slate-500 font-bold uppercase tracking-widest">Discount Per Person</p>
                    </div>
                    <div class="bg-white/5 p-8 rounded-3xl text-center border border-white/10 fade-in hover:bg-white/10 transition-all">
                        <p class="text-amber-400 font-black text-sm mb-4">출발 120일 전</p>
                        <h4 class="text-3xl font-black mb-2">75,000원</h4>
                        <p class="text-[11px] text-slate-500 font-bold uppercase tracking-widest">Discount Per Person</p>
                    </div>
                    <div class="bg-white/5 p-8 rounded-3xl text-center border border-white/10 fade-in hover:bg-white/10 transition-all">
                        <p class="text-amber-400 font-black text-sm mb-4">출발 90일 전</p>
                        <h4 class="text-3xl font-black mb-2">50,000원</h4>
                        <p class="text-[11px] text-slate-500 font-bold uppercase tracking-widest">Discount Per Person</p>
                    </div>
                    <div class="bg-white/5 p-8 rounded-3xl text-center border border-white/10 fade-in hover:bg-white/10 transition-all">
                        <p class="text-amber-400 font-black text-sm mb-4">출발 60일 전</p>
                        <h4 class="text-3xl font-black mb-2">25,000원</h4>
                        <p class="text-[11px] text-slate-500 font-bold uppercase tracking-widest">Discount Per Person</p>
                    </div>
                </div>
                <p class="text-center text-slate-500 text-xs mt-12">* 항공/호텔 사정에 따라 조기 마감될 수 있습니다. (몰디브 등 일부 지역 제외)</p>
            </div>
        </section>

        <!-- Section 4: Gifts (Renamed to 선택 1, 2, 3) -->
        <section id="gift-section" class="py-24 bg-white">
            <div class="container mx-auto px-6 max-w-7xl">
                <div class="section-header">
                    <i class="fas fa-gift"></i>
                    <h2>스페셜 허니문 사은품 선택</h2>
                    <p class="text-gray-500 mt-4 font-medium">행복한 여행의 시작, 아일항공이 준비한 세 가지 특별한 선물 중 하나를 선택하세요.</p>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-3 gap-10 mb-16">
                    <!-- Gift 1 -->
                    <div class="gift-card p-8 rounded-[2.5rem] text-center fade-in relative" onclick="selectGift(1)">
                        <div class="absolute top-6 right-6 text-rose-500 text-2xl hidden check-icon"><i class="fas fa-check-circle"></i></div>
                        <span class="inline-block px-5 py-1.5 bg-[#d4a373] text-white text-xs font-black rounded-full mb-6">선택 1</span>
                        <h3 class="font-black text-2xl mb-2 text-gray-900">허니문 하드케이스 세트</h3>
                        <p class="text-sm text-gray-400 mb-4">실용성과 내구성을 갖춘 25인치+20인치 구성</p>
                        
                        <div class="img-grid grid-4">
                            <div><img src="https://postfiles.pstatic.net/MjAyNjAxMjRfMTAz/MDAxNzY5MjI3MDQ5Nzk3.4p4LrIOvDf8J7-ADbS6K-GcA8pQ-1TjYs03SYJqIVIEg.p45vZsAn2KgTHxjBYRrZ9h6-MuHA6TbZfpvLg8_OX7gg.JPEG/EA-107_%EB%AF%BC%ED%8A%B8.jpg?type=w580" class="img-item"><div class="img-caption">민트</div></div>
                            <div><img src="https://postfiles.pstatic.net/MjAyNjAxMjRfMTcg/MDAxNzY5MjI3MDYxNjIx.TEgvxh6_gfXgDr5yRdhtVXiXvf0YsmIhiB1oCocbEzog.brV8Yd1hVbnkOuGRYHKLXBdScu_x_w-SBVnHUMhVY0Eg.JPEG/EA-107_%EB%B2%A0%EC%9D%B4%EC%A7%80.jpg?type=w580" class="img-item"><div class="img-caption">베이지</div></div>
                            <div><img src="https://postfiles.pstatic.net/MjAyNjAxMjRfMTgw/MDAxNzY5MjI3MDM4OTAx.Qot_Jwki2r-JZyoA7zCR5goqWJmPZW4zDCBuvdxsLjIg.T0seqwkcJCXEWF3sKs_kQxW--1gPlINabujkTOwbofkg.JPEG/EA-107_%EB%84%A4%EC%9D%B4%EB%B9%84.jpg?type=w580" class="img-item"><div class="img-caption">네이비</div></div>
                            <div><img src="https://postfiles.pstatic.net/MjAyNjAxMjRfMTQ4/MDAxNzY5MjI3MDcxODA3.STrv3W579YbcL4tEFhbrCkx5_yO4ozVydqa5ni9W9EQg.OU6c3RCvvJmwX_ePWVn5ObCYZ1fMjWHN2-R4rfWHVx8g.JPEG/EA-107_%EB%B8%8C%EB%9D%BC%EC%9A%B4.jpg?type=w580" class="img-item"><div class="img-caption">브라운</div></div>
                            <div><img src="https://postfiles.pstatic.net/MjAyNjAxMjRfMTcz/MDAxNzY5MjI2OTg2MzE0.aBfYda8qeSwAJPI5wdR-EzCJO64hrzAi5Nl4blmgNPwg.l2tq8_yG4rQp8gnqEVI0VAXsYRBZS6zYCnnyDEpwu5og.JPEG/EA-105_%EB%B8%94%EB%9E%99.jpg?type=w580" class="img-item"><div class="img-caption">블랙</div></div>
                            <div><img src="https://postfiles.pstatic.net/MjAyNjAxMjRfMjk0/MDAxNzY5MjI3MDI1NTMw.hVx5q-NFLTHCgKVOIeikd7j-dS0L2FaFjSP8peSmoJ0g.msgQq_rkGsFLzU_tBX1-K_Uyj2BHVKgOZU1HeQCW_i8g.JPEG/EA-105_%ED%99%94%EC%9D%B4%ED%8A%B8.jpg?type=w580" class="img-item"><div class="img-caption">화이트</div></div>
                            <div><img src="https://postfiles.pstatic.net/MjAyNjAxMjRfMjY0/MDAxNzY5MjI3MDEzOTMx.A6VvHJJ4JarQiYpLCZ4tltx_CUd9tq_o3XaLwwEz8tAg.9mBcOBRjiWWdXrP4kzO3Q_-PJGWFykZnCcCARu6cm1Qg.JPEG/EA-105_%ED%8E%84%EA%B7%B8%EB%A0%88%EC%9D%B4.jpg?type=w580" class="img-item"><div class="img-caption">펄그레이</div></div>
                            <div><img src="https://postfiles.pstatic.net/MjAyNjAxMjRfMjM0/MDAxNzY5MjI3MDAwMjQ3.7fEKSOsWaPB4nHmZlaSKd84LCN-ym4HhxE6okLHFSakg.4Gwo00qYc7Unws28LNGTeJkEmkWHhfETu3RJ0hNGkN4g.JPEG/EA-105_%EC%98%A4%EC%85%98%EB%B8%94%EB%A3%A8.jpg?type=w580" class="img-item"><div class="img-caption">오션블루</div></div>
                        </div>
                        <div class="text-[11px] font-bold py-2 bg-rose-50 text-rose-600 rounded-xl mt-4">🚚 주소지 무료 배송 서비스</div>
                    </div>

                    <!-- Gift 2 -->
                    <div class="gift-card p-8 rounded-[2.5rem] text-center fade-in relative" onclick="selectGift(2)">
                        <div class="absolute top-6 right-6 text-rose-500 text-2xl hidden check-icon"><i class="fas fa-check-circle"></i></div>
                        <span class="inline-block px-5 py-1.5 bg-[#d4a373] text-white text-xs font-black rounded-full mb-6">선택 2</span>
                        <h3 class="font-black text-2xl mb-2 text-gray-900">프리미엄 허니문 앨범</h3>
                        <p class="text-sm text-gray-400 mb-4">소중한 찰나를 영원히, 30x35cm 프리미엄 화보</p>
                        
                        <div class="img-grid grid-3">
                            <div><img src="https://postfiles.pstatic.net/MjAyNjAxMjRfMjk5/MDAxNzY5MjI3MTAzNTg0.gNAgZKA4ZGv2f8IK5WNLu69Y9YC8QUNTJYUPfJYAWiYg.cTjLQBCT-5r6eB514md9TVjLBXPQGuFoa0Nmnx_THi8g.JPEG/Option02_2.jpg?type=w580" class="img-item"><div class="img-caption">내지 상세</div></div>
                            <div><img src="https://postfiles.pstatic.net/MjAyNjAxMjRfODIg/MDAxNzY5MjI3MTE3MDc0.krZJ6syyHA1ldwf3VFHhX9PZMSh5AhweRrIEbZDX0kcg.TKhEvtuHfDUPCn1_uciN4ZferR2d2HJyCxqGW6Vkb24g.JPEG/Option02_3.jpg?type=w580" class="img-item"><div class="img-caption">커버 디자인</div></div>
                            <div><img src="https://postfiles.pstatic.net/MjAyNjAxMjRfMjQ0/MDAxNzY5MjI3MTMzMTM4.GDFLbGcy_mahvQgttLuPkLfmYXaCLuFhz5AUzZiZpvMg.zR173plTHmedahvA6kXuLWnXzFrnQJvDd32vZkCXCw0g.JPEG/Option02_4.jpg?type=w580" class="img-item"><div class="img-caption">구성 안내</div></div>
                        </div>
                        <div class="text-[11px] font-bold py-2 bg-rose-50 text-rose-600 rounded-xl mt-4">📅 사진 제출 후 4주 뒤 수령</div>
                    </div>

                    <!-- Gift 3 -->
                    <div class="gift-card p-8 rounded-[2.5rem] text-center fade-in relative" onclick="selectGift(3)">
                        <div class="absolute top-6 right-6 text-rose-500 text-2xl hidden check-icon"><i class="fas fa-check-circle"></i></div>
                        <span class="inline-block px-5 py-1.5 bg-[#d4a373] text-white text-xs font-black rounded-full mb-6">선택 3</span>
                        <h3 class="font-black text-2xl mb-2 text-gray-900">브랜든 압축 파우치 세트</h3>
                        <p class="text-sm text-gray-400 mb-4">짐 부피를 반으로, 여행의 질을 높이는 아이템</p>
                        
                        <div class="img-grid grid-3">
                            <div><img src="https://postfiles.pstatic.net/MjAyNjAxMjRfMjQy/MDAxNzY5MjI3MTc4OTI3.E0znalstqimwjDdBPCTgi2VClcQY6sbg-NSWRRAdUnMg.wU27whKirs_zq_DnsO1aulicT1QUteMPFTKzHP323gcg.JPEG/Option_03_-2.jpg?type=w580" class="img-item"><div class="img-caption">압축 원리</div></div>
                            <div><img src="https://postfiles.pstatic.net/MjAyNjAxMjRfMTc0/MDAxNzY5MjI3MTk0MTM1.frA7tQAjTib2pT7EWL1a-WLIFDFgbaYRCEQSjADQmrQg.UXutzxO_oV1tZI3Xx43buSIveXVXJ0aK-Y5kGQOWsvgg.JPEG/Option_03_-3.jpg?type=w580" class="img-item"><div class="img-caption">컬러 라인업</div></div>
                            <div><img src="https://postfiles.pstatic.net/MjAyNjAxMjRfMTQw/MDAxNzY5MjI3MjA4NDY5.niiZR4-bLc012XI6ARkWd4KUcVVPzb9GnNJs_npsYdUg.WB-czZvJfPlL6IEuvkwGbhumQz4YilohBlIleqLQ25og.JPEG/Option03_4.jpg?type=w580" class="img-item"><div class="img-caption">활용 예시</div></div>
                        </div>
                        <div class="text-[11px] font-bold py-2 bg-rose-50 text-rose-600 rounded-xl mt-4">🚚 2세대 신형 무료 배송</div>
                    </div>
                </div>

                <!-- Selected Gift Placeholder -->
                <div id="giftForm" class="max-w-4xl mx-auto bg-gray-50 p-10 rounded-[3rem] text-center border border-gray-100 hidden fade-in">
                    <h4 id="selectedGiftTitle" class="text-2xl font-black text-rose-500 mb-6"></h4>
                    <p class="text-gray-500 font-bold">담당자 상담 시 선택하신 사은품을 말씀해 주시면 예약 리스트에 등록해 드립니다.</p>
                </div>
            </div>
        </section>

        <!-- Section 5: Referrals (Title Restored with consistency) -->
        <section id="event-section" class="py-24 bg-blue-50/50">
            <div class="container mx-auto px-6 max-w-6xl">
                <!-- Restored standard section title for Referral -->
                <div class="section-header">
                    <i class="fas fa-handshake"></i>
                    <h2>지인 추천 무제한 혜택</h2>
                    <p class="text-gray-500 mt-4 font-medium">소중한 인연을 아일항공과 함께 나누고 더 큰 혜택을 받으세요.</p>
                </div>

                <div class="bg-white rounded-[4rem] p-10 md:p-16 shadow-2xl flex flex-col md:flex-row items-center gap-12 fade-in">
                    <div class="w-full md:w-1/2 overflow-hidden rounded-[2rem] shadow-xl border-4 border-white bg-white">
                        <img src="https://apgujeong-ticket.co.kr/wp-content/uploads/2018/12/%ED%98%84%EB%8C%80%EB%B0%B1%ED%99%94%EC%A0%9010%EB%A7%8C%EC%9B%90.png" alt="현대백화점 상품권" class="w-full h-auto hover:scale-105 transition-transform duration-1000">
                    </div>
                    <div class="w-full md:w-1/2 space-y-6">
                        <div class="inline-flex items-center gap-2 text-blue-700 font-black text-xs uppercase tracking-tighter">
                            <span class="w-8 h-[1px] bg-blue-700"></span> Recommendation Relay
                        </div>
                        <h2 class="text-3xl font-black text-gray-900 leading-tight">허니문 릴레이<br>지인 추천 혜택</h2>
                        <p class="text-gray-600 leading-relaxed text-lg font-medium">
                            아일항공에서의 행복한 기억을 지인에게 추천해 주세요. 추천받은 지인이 예약을 완료할 때마다 감사의 마음을 담아 <span class="text-blue-600 font-black underline decoration-4 decoration-blue-100 underline-offset-4">현대백화점 10만원 상품권</span>을 즉시 증정합니다. 횟수 제한 없는 놀라운 무제한 혜택을 경험하세요!
                        </p>
                    </div>
                </div>
            </div>
        </section>

        <!-- Section 6: Reviews -->
        <section id="review-section" class="py-24 bg-rose-50 overflow-hidden">
            <div class="container mx-auto px-6 max-w-7xl">
                <div class="max-w-4xl mx-auto text-center mb-16 fade-in">
                    <span class="text-rose-500 font-black text-xs uppercase tracking-widest block mb-4">Memory archiving</span>
                    <h2 class="text-4xl font-black text-rose-600 mb-6 leading-tight">기억은 기록할 때<br><span class="underline decoration-rose-200 underline-offset-8">추억이 됩니다</span></h2>
                    <p class="text-gray-500 text-lg font-medium">정성스러운 여행 후기를 남겨주시는 모든 아일항공 가족 여러분께<br><span class="font-black text-gray-900">30인치 프리미엄 아크릴 액자</span>를 100% 선물합니다.</p>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-3 gap-8 mb-16 max-w-5xl mx-auto">
                    <div class="bg-white p-10 rounded-[3rem] shadow-sm text-center fade-in hover:shadow-xl transition-shadow">
                        <div class="w-16 h-16 bg-rose-100 text-rose-600 rounded-3xl flex items-center justify-center mx-auto mb-6 text-2xl font-black rotate-6">01</div>
                        <h4 class="font-black text-xl mb-3 text-gray-900">인생샷 준비</h4>
                        <p class="text-sm text-gray-400 font-medium leading-relaxed">여행지의 행복한 순간이 담긴 사진 10컷 이상을 선별해 주세요.</p>
                    </div>
                    <div class="bg-white p-10 rounded-[3rem] shadow-sm text-center fade-in hover:shadow-xl transition-shadow">
                        <div class="w-16 h-16 bg-blue-100 text-blue-600 rounded-3xl flex items-center justify-center mx-auto mb-6 text-2xl font-black -rotate-6">02</div>
                        <h4 class="font-black text-xl mb-3 text-gray-900">후기 업로드</h4>
                        <p class="text-sm text-gray-400 font-medium leading-relaxed">지정된 카페 혹은 개인 블로그에 정성 가득한 후기를 작성합니다.</p>
                    </div>
                    <div class="bg-white p-10 rounded-[3rem] shadow-sm text-center fade-in hover:shadow-xl transition-shadow">
                        <div class="w-16 h-16 bg-amber-100 text-amber-600 rounded-3xl flex items-center justify-center mx-auto mb-6 text-2xl font-black rotate-3">03</div>
                        <h4 class="font-black text-xl mb-3 text-gray-900">베스트 컷 제출</h4>
                        <p class="text-sm text-gray-400 font-medium leading-relaxed">액자로 제작할 최고의 사진 1장을 담당자에게 보내면 끝!</p>
                    </div>
                </div>

                <div class="text-center fade-in">
                    <a href="https://xn--ob0bx78a0kditas4d378axfa.com/bbs/board.php?bo_table=a_recom" target="_blank" class="inline-block px-16 py-6 bg-rose-600 text-white rounded-full font-black text-xl shadow-2xl hover:scale-105 active:scale-95 transition-all">
                        후기 작성하고 선물 신청하기
                    </a>
                </div>
            </div>
        </section>

        <!-- Section 7: Honeymoon Concierge (4x1 Grid) -->
        <section class="py-24 bg-white border-t border-gray-50">
            <div class="container mx-auto px-6 text-center max-w-7xl">
                <div class="section-header">
                    <i class="fas fa-headset"></i>
                    <h2>Honeymoon Concierge</h2>
                    <p class="text-gray-400 mt-2 font-medium">전문가와 상담하여 더욱 완벽한 허니문을 계획하세요.</p>
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8 max-w-7xl mx-auto">
                    <!-- 1. 대표 전화 -->
                    <a href="tel:043-250-7777" class="group p-10 rounded-[3rem] bg-gray-50 hover:bg-amber-50 transition-all border border-transparent hover:border-amber-200 shadow-sm hover:shadow-xl">
                        <div class="w-16 h-16 bg-white rounded-2xl flex items-center justify-center mx-auto mb-6 shadow-sm group-hover:scale-110 transition-transform">
                            <i class="fas fa-phone-alt text-2xl text-amber-700"></i>
                        </div>
                        <h4 class="font-black text-xl mb-2 text-gray-900">대표 전화 상담</h4>
                        <p class="text-sm text-gray-500 font-bold">043-250-7777</p>
                    </a>
                    <!-- 2. 카카오톡 -->
                    <a href="http://pf.kakao.com/_xlxaXBs/chat" target="_blank" class="group p-10 rounded-[3rem] bg-gray-50 hover:bg-yellow-50 transition-all border border-transparent hover:border-yellow-200 shadow-sm hover:shadow-xl">
                        <div class="w-16 h-16 bg-white rounded-2xl flex items-center justify-center mx-auto mb-6 shadow-sm group-hover:scale-110 transition-transform">
                            <i class="fas fa-comment text-2xl text-yellow-500"></i>
                        </div>
                        <h4 class="font-black text-xl mb-2 text-gray-900">카카오톡 채널</h4>
                        <p class="text-sm text-gray-500 font-bold">아일항공여행사</p>
                    </a>
                    <!-- 3. 네이버 예약 -->
                    <a href="https://map.naver.com/p/search/%EC%95%84%EC%9D%BC%ED%95%AD%EA%B3%B5%EC%97%AC%ED%96%89%EC%82%AC/place/1983566624" target="_blank" class="group p-10 rounded-[3rem] bg-gray-50 hover:bg-green-50 transition-all border border-transparent hover:border-green-200 shadow-sm hover:shadow-xl">
                        <div class="w-16 h-16 bg-white rounded-2xl flex items-center justify-center mx-auto mb-6 shadow-sm group-hover:scale-110 transition-transform">
                            <i class="fas fa-calendar-check text-2xl text-green-600"></i>
                        </div>
                        <h4 class="font-black text-xl mb-2 text-gray-900">방문 상담 예약</h4>
                        <p class="text-sm text-gray-500 font-bold">네이버 공식 예약</p>
                    </a>
                    <!-- 4. 공식 블로그 (Fixed Icon) -->
                    <a href="https://blog.naver.com/travelcek" target="_blank" class="group p-10 rounded-[3rem] bg-gray-50 hover:bg-emerald-50 transition-all border border-transparent hover:border-emerald-200 shadow-sm hover:shadow-xl">
                        <div class="w-16 h-16 bg-white rounded-2xl flex items-center justify-center mx-auto mb-6 shadow-sm group-hover:scale-110 transition-transform">
                            <i class="fas fa-feather-pointed text-2xl text-emerald-600"></i>
                        </div>
                        <h4 class="font-black text-xl mb-2 text-gray-900">공식 블로그</h4>
                        <p class="text-sm text-gray-500 font-bold">여행 정보 & 스토리</p>
                    </a>
                </div>
            </div>
        </section>
    </div>

    <!-- Section 8: Footer (Fixed Naver Icon) -->
    <footer class="bg-[#1e272e] py-24 px-6 text-white overflow-hidden relative">
        <div class="absolute -right-20 -top-20 w-80 h-80 bg-white/5 rounded-full blur-3xl"></div>
        
        <div class="container mx-auto max-w-6xl relative z-10">
            <div class="flex flex-col lg:flex-row justify-between items-start gap-16 border-b border-white/10 pb-16">
                <div class="max-w-md">
                    <h4 class="text-3xl font-black mb-8 italic tracking-tighter">아일항공여행사</h4>
                    <p class="text-slate-400 text-[15px] leading-relaxed mb-8">아일항공여행사는 정직과 신뢰를 바탕으로 가장 완벽한 허니문을 설계합니다. 인생의 새로운 시작을 함께하는 든든한 파트너가 되겠습니다.</p>
                    <div class="flex gap-4">
                        <a href="https://blog.naver.com/travelcek" target="_blank" title="네이버 블로그" class="w-10 h-10 rounded-full bg-white/10 flex items-center justify-center hover:bg-[#2DB400] transition-colors">
                            <i class="fas fa-pen-nib text-sm"></i>
                        </a>
                        <a href="http://pf.kakao.com/_xlxaXBs/chat" target="_blank" title="카카오톡" class="w-10 h-10 rounded-full bg-white/10 flex items-center justify-center hover:bg-[#FEE500] hover:text-black transition-colors">
                            <i class="fas fa-comment text-sm"></i>
                        </a>
                    </div>
                </div>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-12 text-sm text-slate-300">
                    <div class="space-y-4">
                        <p class="text-white font-black text-lg mb-2 uppercase tracking-wide">Corporate Info</p>
                        <p><span class="text-slate-500 mr-2">대표이사 :</span> 이경수</p>
                        <p><span class="text-slate-500 mr-2">사업자번호 :</span> 301-81-32215</p>
                        <p><span class="text-slate-500 mr-2">본점주소 :</span> 충북 청주시 상당구 남사로 93번길 5-1</p>
                    </div>
                    <div class="space-y-4">
                        <p class="text-white font-black text-lg mb-2 uppercase tracking-wide">Contact Channels</p>
                        <p><span class="text-slate-500 mr-2">대표번호 :</span> 043-250-7777</p>
                        <p><span class="text-slate-500 mr-2">FAX :</span> 043-250-7779</p>
                        <p><span class="text-slate-500 mr-2">상담채널 :</span> 담당자 실시간 상담 채널 운영</p>
                    </div>
                </div>
            </div>
            
            <div class="mt-12 flex flex-col md:flex-row justify-between items-center gap-6 text-[11px] text-slate-500 font-bold tracking-widest uppercase">
                <div>&copy; 2024 AIL AIR TRAVEL SERVICE. ALL RIGHTS RESERVED.</div>
                <div class="flex gap-6">
                    <a href="#" class="hover:text-white transition-colors">Privacy Policy</a>
                    <a href="#" class="hover:text-white transition-colors">Terms of Service</a>
                </div>
            </div>
        </div>
    </footer>

    <!-- Scripts -->
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/@elfalem/leaflet-curve@0.9.2/dist/leaflet.curve.min.js"></script>

    <script>
        /* 1. Map Initialization (31 destinations preserved) */
        const ICN = { lat: 37.4602, lng: 126.4407 };
        const destinations = [
            { name: "나트랑", lat: 12.2388, lng: 109.1967 }, { name: "푸꾸옥", lat: 10.2289, lng: 103.9572 },
            { name: "푸켓", lat: 7.8804, lng: 98.3923 }, { name: "크라비", lat: 8.0863, lng: 98.9063 },
            { name: "코사무이", lat: 9.5120, lng: 100.0136 }, { name: "카오락", lat: 8.6439, lng: 98.2443 },
            { name: "코리뻬", lat: 6.4883, lng: 99.3025 }, { name: "괌", lat: 13.4443, lng: 144.7937 },
            { name: "사이판", lat: 15.1900, lng: 145.7500 }, { name: "발리", lat: -8.4095, lng: 115.1889 },
            { name: "두바이", lat: 25.2048, lng: 55.2708 }, { name: "하와이", lat: 21.3069, lng: -157.8583 },
            { name: "몰디브", lat: 3.2028, lng: 73.2207 }, { name: "모리셔스", lat: -20.3484, lng: 57.5522 },
            { name: "파리", lat: 48.8566, lng: 2.3522 }, { name: "스페인", lat: 40.4168, lng: -3.7038 },
            { name: "포르투갈", lat: 38.7223, lng: -9.1393 }, { name: "스위스", lat: 46.9480, lng: 7.4474 },
            { name: "이탈리아", lat: 41.9028, lng: 12.4964 }, { name: "체코", lat: 50.0755, lng: 14.4378 },
            { name: "오스트리아", lat: 48.2082, lng: 16.3738 }, { name: "헝가리", lat: 47.4979, lng: 19.0402 },
            { name: "독일", lat: 52.5200, lng: 13.4050 }, { name: "칸쿤", lat: 21.1619, lng: -86.8515 },
            { name: "뉴욕", lat: 40.7128, lng: -74.0060 }, { name: "시드니", lat: -33.8688, lng: 151.2093 },
            { name: "골드코스트", lat: -28.0167, lng: 153.4000 }, { name: "케언즈", lat: -16.9186, lng: 145.7710 },
            { name: "오클랜드", lat: -36.8485, lng: 174.7633 }, { name: "이스탄불", lat: 41.0082, lng: 28.9784 },
            { name: "캐나다", lat: 43.6532, lng: -79.3832 }
        ];

        const map = L.map('map', { 
            center: [20, 150], 
            zoom: 3, 
            minZoom: 2, 
            worldCopyJump: true,
            scrollWheelZoom: false 
        });
        
        L.tileLayer('https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png', {
            attribution: '&copy; CARTO'
        }).addTo(map);

        const icnIcon = L.divIcon({ className: 'marker-dot icn-dot', iconSize: [16, 16], iconAnchor: [8, 8] });
        L.marker([ICN.lat, ICN.lng], { icon: icnIcon }).bindTooltip("인천 (ICN)", { 
            permanent: true, 
            direction: 'bottom', 
            className: 'area-label', 
            offset: [0, 10] 
        }).addTo(map);

        destinations.forEach(dest => {
            let targetLng = dest.lng;
            if (dest.lng - ICN.lng > 180) targetLng -= 360;
            if (dest.lng - ICN.lng < -180) targetLng += 360;

            const dot = L.divIcon({ className: 'marker-dot', iconSize: [12, 12], iconAnchor: [6, 6] });
            L.marker([dest.lat, targetLng], { icon: dot }).bindTooltip(dest.name, { 
                permanent: true, 
                direction: 'top', 
                className: 'area-label', 
                offset: [0, -10] 
            }).addTo(map);

            const midLat = (ICN.lat + dest.lat) / 2 + (Math.abs(targetLng - ICN.lng) * 0.15);
            const midLng = (ICN.lng + targetLng) / 2;
            L.curve(['M', [ICN.lat, ICN.lng], 'Q', [midLat, midLng], [dest.lat, targetLng]], {
                color: '#ff4757', weight: 1.5, opacity: 0.25, dashArray: '6, 6', fill: false
            }).addTo(map);
        });

        /* 2. Gift Selection Logic */
        let currentGift = 0;
        function selectGift(num) {
            currentGift = num;
            document.querySelectorAll('.gift-card').forEach(c => c.classList.remove('selected'));
            document.querySelectorAll('.check-icon').forEach(i => i.classList.add('hidden'));
            
            const cards = document.querySelectorAll('.gift-card');
            cards[num-1].classList.add('selected');
            cards[num-1].querySelector('.check-icon').classList.remove('hidden');
            
            const form = document.getElementById('giftForm');
            const title = document.getElementById('selectedGiftTitle');
            form.classList.remove('hidden');
            
            const titles = ["하드케이스 캐리어 세트", "프리미엄 허니문 앨범", "브랜든 압축 파우치 세트"];
            title.innerHTML = `<i class="fas fa-check-circle mr-2"></i> "선택 ${num}: ${titles[num-1]}" 선택 완료`;
            
            setTimeout(() => form.scrollIntoView({ behavior: 'smooth', block: 'center' }), 100);
        }

        /* 3. Interaction & Animation */
        const observerOptions = { threshold: 0.1 };
        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.classList.add('visible');
                }
            });
        }, observerOptions);

        document.querySelectorAll('.fade-in').forEach(el => observer.observe(el));

        // Sticky Navbar Effect
        window.addEventListener('scroll', () => {
            const nav = document.querySelector('nav');
            if (window.scrollY > 50) {
                nav.classList.add('py-2', 'shadow-md');
                nav.classList.remove('py-3', 'shadow-sm');
            } else {
                nav.classList.add('py-3', 'shadow-sm');
                nav.classList.remove('py-2', 'shadow-md');
            }
        });
    </script>
</body>
</html>
