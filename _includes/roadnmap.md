<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap" rel="stylesheet">
<style>
    .roadmap-wrapper {
        font-family: 'Inter', sans-serif;
        color: #334155;
        position: relative;
        /* 平时：宽度默认 100%，与上下文文本宽度一致 */
        width: 90%;
        margin: 1.5rem auto 2.5rem auto;
    }
    
    /* ========== 全屏悬浮展现状态 ========== */
    .roadmap-wrapper.is-expanded {
        position: fixed;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        max-width: none;
        margin: 0;
        padding: 0;
        z-index: 999999; /* 保证铺满全屏并在所有元素之上 */
        background: rgba(248, 250, 252, 0.96);
        backdrop-filter: blur(8px); /* 加点模糊效果更现代 */
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .roadmap-wrapper .roadmap-card {
        background: #ffffff;
        border-radius: 20px;
        box-shadow: 0 10px 40px rgba(0, 0, 0, 0.04);
        border: 1px solid #f1f5f9;
        position: relative;
        padding: 5px; 
        width: 100%;
        overflow: hidden; /* 隐藏最外层溢出，保证右上角按钮不会随内容滑动 */
    }

    .roadmap-wrapper.is-expanded .roadmap-card {
        width: 100vw;
        height: 100vh;
        border-radius: 0;
        border: none;
        box-shadow: none;
        background: transparent;
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 0;
    }

    /* 真正的滑动区域放在 card 里面 */
    .roadmap-wrapper .roadmap-scroll-area {
        width: 100%;
        overflow-x: auto;
        padding: 55px 15px 15px 15px; /* 留出顶部给右上角按钮 */
    }

    /* 展开时：绝对锁定尺寸，禁止任何滑动（不能左右上下移动） */
    .roadmap-wrapper.is-expanded .roadmap-scroll-area {
        overflow: hidden !important; 
        padding: 0;
        display: flex;
        align-items: center;
        justify-content: center;
        width: 100vw;
        height: 100vh;
    }

    /* 缩放物理占位盒 */
    .roadmap-wrapper .roadmap-scale-wrapper {
        /* 平时：缩放至 0.65，默认宽度占用符合小图模式 */
        width: 754px; /* 1160 * 0.65 */
        height: 468px; /* 720 * 0.65 */
        margin: 0 auto;
    }
    .roadmap-wrapper.is-expanded .roadmap-scale-wrapper {
        width: 1160px;
        height: 720px;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .roadmap-wrapper .roadmap-container {
        position: relative;
        width: 1160px; /* 原图宽度 */
        height: 720px; /* 原图高度 */
        transform: scale(0.65);
        transform-origin: top left;
        transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
    }
    /* 全屏放大时改变缩放原点为中心点 */
    .roadmap-wrapper.is-expanded .roadmap-container {
        transform-origin: center center; 
    }

    /* 扩展/缩小 按钮样式，固定在右上角 */
    .roadmap-toggle-btn {
        position: absolute;
        top: 15px;
        right: 15px;
        background: #f8fafc;
        border: 1px solid #e2e8f0;
        border-radius: 8px;
        padding: 6px 12px;
        font-family: 'Inter', sans-serif;
        font-size: 13px;
        font-weight: 700;
        color: #475569;
        cursor: pointer;
        z-index: 50;
        transition: all 0.2s;
        display: flex;
        align-items: center;
        gap: 6px;
        box-shadow: 0 2px 6px rgba(0,0,0,0.05);
        outline: none;
    }
    .roadmap-wrapper.is-expanded .roadmap-toggle-btn {
        top: 25px;
        right: 25px;
        box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        border-color: #cbd5e1;
    }
    .roadmap-toggle-btn:hover {
        background: #f1f5f9;
        color: #0f172a;
        transform: translateY(-1px);
        box-shadow: 0 4px 10px rgba(0,0,0,0.08);
        border-color: #cbd5e1;
    }
    .roadmap-toggle-btn svg {
        transition: transform 0.3s ease;
    }
    
    .roadmap-wrapper .axis-label {
        position: absolute;
        font-size: 13.5px;
        font-weight: 600;
        color: #64748b;
        line-height: 1.4;
    }
    .roadmap-wrapper .axis-title {
        position: absolute;
        font-weight: 800;
        font-size: 15.5px;
        color: #1e293b;
    }
    
    .roadmap-wrapper .paper-node {
        position: absolute;
        transform: translate(-50%, -50%);
        text-align: center;
        font-size: 11px;
        line-height: 1.35;
        width: max-content; 
        white-space: nowrap;
        background: #ffffff;
        border-radius: 12px;
        padding: 8px 10px;
        box-shadow: 0 4px 15px rgba(0,0,0,0.06);
        border: 1px solid #e2e8f0;
        transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        z-index: 10;
    }
    .roadmap-wrapper .paper-node:hover {
        transform: translate(-50%, -50%) scale(1.08); 
        box-shadow: 0 12px 30px rgba(236, 72, 153, 0.15); 
        border-color: #fbcfe8;
        z-index: 20;
        cursor: pointer;
    }
    .roadmap-wrapper .paper-node strong {
        color: #0f172a; 
        display: block;
        margin-bottom: 5px;
        font-size: 12.5px;
        font-weight: 800;
        line-height: 1.35;
    }
    
    .roadmap-wrapper .target-node {
        position: absolute;
        transform: translate(-50%, -50%);
        background: linear-gradient(135deg, #ec4899 0%, #8b5cf6 100%);
        color: white;
        font-weight: 700;
        padding: 14px 20px;
        border-radius: 14px;
        width: max-content; 
        white-space: nowrap;
        text-align: center;
        font-size: 14.5px;
        line-height: 1.4;
        box-shadow: 0 8px 25px rgba(236, 72, 153, 0.35);
        z-index: 15;
        border: 2px solid rgba(255,255,255,0.3);
        transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    }
    .roadmap-wrapper .target-node:hover {
        transform: translate(-50%, -50%) scale(1.04);
        box-shadow: 0 12px 35px rgba(236, 72, 153, 0.5);
        cursor: pointer;
    }

    .roadmap-wrapper .svg-layer {
        position: absolute;
        top: 0;
        left: 0;
        z-index: 1;
        pointer-events: none; 
    }

    .roadmap-wrapper .has-text-centered {
        text-align: center !important;
    }
</style>

<div class="roadmap-wrapper" id="roadmap-wrapper">
    <div class="roadmap-card">
        
        <!-- Toggle Expand Button -->
        <button class="roadmap-toggle-btn" id="roadmap-btn" onclick="toggleRoadmap()" aria-label="Toggle full screen map">
            <svg id="roadmap-icon-expand" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M15 3h6v6"/><path d="M9 21H3v-6"/><path d="M21 3l-7 7"/><path d="M3 21l7-7"/></svg>
            <svg id="roadmap-icon-collapse" style="display:none;" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M4 14h6v6"/><path d="M20 10h-6V4"/><path d="M14 10l7-7"/><path d="M3 21l7-7"/></svg>
            <span id="roadmap-btn-text">Expand</span>
        </button>

        <div class="roadmap-scroll-area">
            <div class="roadmap-scale-wrapper">
                <div class="roadmap-container" id="roadmap-container">
                
                <!-- SVG Background Lines -->
                <svg class="svg-layer" width="1160" height="720" viewBox="0 0 1160 720">
                    <defs>
                        <marker id="arrow" markerWidth="10" markerHeight="10" refX="8" refY="4" orient="auto" markerUnits="strokeWidth">
                            <path d="M0,0 L0,8 L8,4 z" fill="#1e293b" />
                        </marker>
                    </defs>
                    
                    <!-- Main Axes -->
                    <line x1="150" y1="650" x2="1100" y2="650" stroke="#1e293b" stroke-width="3" marker-end="url(#arrow)" />
                    <line x1="150" y1="650" x2="150" y2="50" stroke="#1e293b" stroke-width="3" marker-end="url(#arrow)" />

                    <!-- Horizontal Grid Lines -->
                    <line x1="150" y1="583" x2="1100" y2="583" stroke="#f1f5f9" stroke-width="2" stroke-dasharray="6,6" />
                    <line x1="150" y1="416" x2="1100" y2="416" stroke="#f1f5f9" stroke-width="2" stroke-dasharray="6,6" />
                    <line x1="150" y1="250" x2="1100" y2="250" stroke="#f1f5f9" stroke-width="2" stroke-dasharray="6,6" />
                    <line x1="150" y1="116" x2="1100" y2="116" stroke="#f1f5f9" stroke-width="2" stroke-dasharray="6,6" />

                    <!-- Vertical Dashed Dividers -->
                    <line x1="501" y1="650" x2="501" y2="50" stroke="#cbd5e1" stroke-width="2.5" stroke-dasharray="8,8" />
                    <line x1="872" y1="650" x2="872" y2="50" stroke="#cbd5e1" stroke-width="2.5" stroke-dasharray="8,8" />
                </svg>

                <!-- Axis Titles -->
                <div class="axis-title" style="left: 150px; top: 35px; transform: translate(-50%, -100%); text-align: center; width: 250px;">Robustness<br>Challenge</div>
                <div class="axis-title" style="left: 1110px; top: 650px; transform: translateY(-50%); text-align: left; width: 120px;">Medical<br>Intelligence</div>

                <!-- Y-Axis Region Labels -->
                <div class="axis-label" style="left: 0; width: 135px; top: 583px; transform: translateY(-50%); text-align: right;">Static External<br>Environments</div>
                <div class="axis-label" style="left: 0; width: 135px; top: 416px; transform: translateY(-50%); text-align: right;">Non-stationary<br>Open World</div>
                <div class="axis-label" style="left: 0; width: 135px; top: 250px; transform: translateY(-50%); text-align: right;">Endogenous<br>Hallucinations</div>
                <div class="axis-label" style="left: 0; width: 135px; top: 116px; transform: translateY(-50%); text-align: right;">Precision<br>Personalization</div>

                <!-- X-Axis Region Labels -->
                <div class="axis-label" style="left: 335px; width: 180px; top: 665px; transform: translateX(-50%); text-align: center;">Specialized<br>Model</div>
                <div class="axis-label" style="left: 706px; width: 180px; top: 665px; transform: translateX(-50%); text-align: center;">Reasoning<br>Large Model</div>
                <div class="axis-label" style="left: 965px; width: 180px; top: 665px; transform: translateX(-50%); text-align: center;">Multi-Agent<br>Collaboration</div>

                <!-- Research Paper Nodes -->
                <!-- Specialized Model Area -->
                <div class="paper-node" style="left: 211px; top: 583px;">
                    <strong>BIBM'22</strong>Cross-modal<br>Medical Image<br>Generation
                </div>
                
                <div class="paper-node" style="left: 321px; top: 556px;">
                    <strong>AAAI'23</strong>Long-tailed<br>Pancreatic Tumor<br>Representation
                </div>
                
                <div class="paper-node" style="left: 439px; top: 490px;">
                    <strong>TMI'25</strong>Geometry-Aware<br>Coronary Artery<br>Segmentation
                </div>
                
                <!-- Reasoning Large Model Area -->
                <div class="paper-node" style="left: 636px; top: 530px;">
                    <strong>ICML'25</strong>Efficient<br>Medical Image<br>Representation
                </div>
                
                <div class="paper-node" style="left: 570px; top: 430px;">
                    <strong>AAAI'25 (Oral)</strong>Multi-expert<br>Knowledge Fusion
                </div>
                
                <div class="paper-node" style="left: 692px; top: 356px;">
                    <strong>ICLR'25</strong>Image-Text<br>Alignment in Medical<br>MLLM Pre-training
                </div>
                
                <!-- Temporal Domain -->
                <div class="paper-node" style="left: 765px; top: 450px;">
                    <strong>NeurIPS'25</strong>Medical Temporal<br>Domain Generalization
                </div>
                
                <div class="paper-node" style="left: 793px; top: 250px;">
                    <strong>NeurIPS'25</strong>Chest X-ray<br>Trustworthy Inference<br>Diagnosis
                </div>
                
                <!-- Multi-Agent Area / Under Review / TFS -->
                <div class="paper-node" style="left: 931px; top: 283px;">
                    <strong>(TFS'26)</strong>Multi-Stream<br>Dynamic Data<br>Generalization
                </div>
                
                <div class="paper-node" style="left: 935px; top: 170px;">
                    <strong>(Under Review)</strong>Multiple MLLMs<br>Collaborative<br>Diagnosis
                </div>

                <!-- Target / Future Goal Node -->
                <div class="target-node" style="left: 1001px; top: 69px;">
                    Target: Robust AI for<br>Dynamic Healthcare
                </div>
                
                </div>
            </div>
        </div>
    </div>
</div>

<script>
function fitRoadmapToScreen() {
    const wrapper = document.getElementById('roadmap-wrapper');
    if (!wrapper.classList.contains('is-expanded')) return;
    
    const container = document.getElementById('roadmap-container');
    
    // 我们保留一点边缘空白 (0.95)，确保图表不完全贴边
    const availableWidth = window.innerWidth * 0.95; 
    const availableHeight = window.innerHeight * 0.95;
    
    // 图表画布标准原始尺寸为 1160x720
    const scaleX = availableWidth / 1160;
    const scaleY = availableHeight / 720;
    
    // 取最小值保证能完全存入视口中，不会溢出也不会引发滚动条
    const fitScale = Math.min(scaleX, scaleY);
    
    // 强制赋予绝对缩放并剧中
    container.style.transform = `scale(${fitScale})`;
}

function toggleRoadmap() {
    const wrapper = document.getElementById('roadmap-wrapper');
    const text = document.getElementById('roadmap-btn-text');
    const iconExpand = document.getElementById('roadmap-icon-expand');
    const iconCollapse = document.getElementById('roadmap-icon-collapse');
    const container = document.getElementById('roadmap-container');

    wrapper.classList.toggle('is-expanded');

    if (wrapper.classList.contains('is-expanded')) {
        // 彻底锁死浏览器背景滑动
        document.body.style.overflow = 'hidden';
        
        text.textContent = 'Collapse';
        iconExpand.style.display = 'none';
        iconCollapse.style.display = 'block';

        fitRoadmapToScreen();
    } else {
        // 恢复正常滑动和界面
        document.body.style.overflow = '';
        text.textContent = 'Expand';
        iconExpand.style.display = 'block';
        iconCollapse.style.display = 'none';
        
        // 恢复小图样式中的默认缩放比
        container.style.transform = 'scale(0.65)'; 
    }
}

// 若用户在全屏状态下缩放窗口大小，保证图表自适应重绘不发生溢出
window.addEventListener('resize', fitRoadmapToScreen);
</script>
