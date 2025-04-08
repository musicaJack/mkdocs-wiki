<!DOCTYPE html>
<html>
<head>
<style>
:root {
  --timeline-color: linear-gradient(180deg, #4FC3F7 0%, #66BB6A 100%);
  --phase-color-1: #4FC3F7;
  --phase-color-2: #D4E157;
  --phase-color-3: #FF7043;
  --phase-color-4: #AB47BC;
}

.roadmap-container {
  max-width: 1200px;
  margin: 50px auto;
  position: relative;
  padding: 60px 0;
}

.timeline {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  width: 8px;
  height: calc(100% - 120px);
  background: var(--timeline-color);
  border-radius: 2px;
}

.phase-card {
  width: 45%;
  padding: 20px;
  margin: 30px 0;
  border-radius: 8px;
  background: rgba(255,255,255,0.95);
  box-shadow: 0 8px 32px rgba(31, 38, 135, 0.15);
  backdrop-filter: blur(4px);
  position: relative;
  transition: transform 0.3s;
}

.phase-card:hover {
  transform: translateY(-5px);
}

.phase-card.left {
  margin-right: 55%;
  border-left: 4px solid var(--phase-color-1);
}

.phase-card.right {
  margin-left: 55%;
  border-right: 4px solid var(--phase-color-1);
}

/* 动态时间节点标记 */
.timeline-marker {
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 24px;
  height: 24px;
  border-radius: 50%;
  background: white;
  border: 3px solid;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}

/* 阶段颜色编码 */
.phase-1 .timeline-marker { border-color: var(--phase-color-1); }
.phase-2 .timeline-marker { border-color: var(--phase-color-2); }
.phase-3 .timeline-marker { border-color: var(--phase-color-3); }
.phase-4 .timeline-marker { border-color: var(--phase-color-4); }

.phase-header {
  border-left: 4px solid;
  padding-left: 15px;
  margin: 15px 0;
}

.phase-title {
  font-size: 0.0em;
  color: #2c3e50;
  margin: 0;
}

.phase-period {
  color: #7f8c8d;
  font-size: 0.9em;
}

.tech-section {
  margin: 20px 0;
}

.tech-label {
  font-weight: 600;
  color: #34495e;
  font-size: 0.95em;
  margin: 15px 0 10px;
}

.tech-detail {
  line-height: 1.8;
  color: #566573;
  font-size: 0.8em;
  padding-left: 10px;
}

.tech-highlight {
  display: inline-block;
  padding: 4px 12px;
  border-radius: 20px;
  background: #f0f4f8;
  margin: 5px 0;
  font-size: 0.9em;
}

/*新增图片样式*/
.product-images {
  margin: 25px 0;
  display: flex;
  gap: 15px;
  flex-wrap: wrap;
}
.product-image {
  flex: 1 1 300px;
  max-width: 100%;
  border-radius: 6px;
  border: 1px solid rgba(0,0,0,0.1);
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

@media (max-width: 768px) {
   .product-images {
    flex-direction: column;
  }
  .product-image {
    flex: 1 1 auto;
  }
  .phase-card {
    width: 90%;
    margin: 30px auto !important;
  }
  .timeline {
    left: 20px;
  }
}
</style>
</head>
<body>

<div class="roadmap-container">
  <div class="timeline"></div>

  <!-- Phase 1 -->
  <div class="phase-card left phase-1">
    <div class="timeline-marker" style="top:20px;"></div>
    <div class="phase-header" style="border-color: var(--phase-color-1);">
      <div class="phase-period">2024年10月</div>
      <h2 class="phase-title">概念验证</h2>
    </div>
    
    <div class="tech-section">
      <div class="tech-label">技术洞察</div>
      <div class="tech-detail">
        <p>▪ 多MCU兼容架构设计<br>
           ▪ ESP32/RP2040/LuckFox异构计算<br>
           ▪ 模块化硬件平台构想<br>
        </p>
      </div>
    </div>

    <div class="tech-section">
      <div class="phase-header" style="border-color: var(--phase-color-1);">
        <div class="phase-period">2024年12月</div>
        <div class="tech-label">原型验证</div>
      </div>
      <div class="tech-detail">
        <p>▪ MicroPython运行时适配<br>
           ▪ GPIO/I2C/UART抽象层建立<br>           
           ▪ WGS84坐标系统转换支持（适配百度地图）<br>
           ▪ MQTT协议栈集成 </p>
           <span class="tech-highlight" style="background:#E8F5E9;">实现GPS坐标采集和运动轨迹回放</span><br>
           
      </div>
    </div>
  </div>

  <!-- Phase 2 -->
  <div class="phase-card right phase-2">
    <div class="timeline-marker" style="top:20px;"></div>
    <div class="phase-header" style="border-color: var(--phase-color-2);">
      <div class="phase-period">2025年1月</div>
      <h2 class="phase-title">最小可行产品开发</h2>
    </div>

    <div class="tech-section">
      <div class="tech-label">硬件架构</div>
      <div class="tech-detail">
        <p>▪ RP2040 + Quectel Lc76G + Quectel EC600E LTE4模组_+ 0.96' OLED SSD1306<br>
           <span class="tech-highlight" style="background:#F3E5F5;">WGS坐标转换、MQTT协议栈集成和验证</span></p>
      </div>
    </div>

    <div class="tech-section">
      <div class="tech-label">系统集成</div>
      <div class="tech-detail">
        <p>▪ 服务开发，MQTT协议栈验证<br>
           ▪ 数据传输，端到端延迟&lt;200ms验证<br>
           ▪ 尝试通过标准接口接入外设(I2C、串口)<br>
           ▪ 0.96寸OLED驱动框架验证</p>
      </div>
    </div>
    <!-- 新增图片展示区 -->
    <div class="product-images">
        <img src="/wiki/assets/mvp-prototype1.jpg"
          alt="MVP原型机实物2"
          class="product-image"
          title="采用RP2040主控的工程样机">
        <img src="/wiki/assets/location_track.png"
          alt="轨迹回放"
          class="product-image"
          title="采用RP2040主控的工程样机">
    </div>
  </div>

  <!-- 其他阶段类似结构... -->
  <!-- Phase 3 工程样机开发（2025Q2）-->
  <div class="phase-card left phase-3">
    <div class="timeline-marker" style="top:20px;"></div>
    <div class="phase-header" style="border-color: var(--phase-color-3);">
      <div class="phase-period">2025年1月</div>
      <h2 class="phase-title">工程样机开发</h2>
    </div>
    
    <div class="tech-section">
      <div class="tech-label">硬件设计</div>
      <div class="tech-detail">
        <p>▪ 单层HDI PCB设计<br>
           <span class="tech-highlight">电源域隔离技术</span><br>
           ▪ Type-C PD供电（3-5V输入）<br>
           <span class="tech-highlight">I2C,串口,SPI扩展接口预留</span></p>
      </div>
    </div>

    <div class="tech-section">
      <div class="tech-label">样机验证（EVT）</div>
      <div class="tech-detail">
        <p>▪ 8台工程样机制作<br>
           ▪ 环境可靠性测试：<br>
           &nbsp;&nbsp;- 25℃/60%RH 72小时老化<br>
           &nbsp;&nbsp;- 多场景信号测试<br>
           <span class="tech-highlight" style="background:#FFEBEE;">EMI/EMC预认证</span></p>
      </div>
    </div>
  </div>

  <!-- Phase 4 平台化架构设计（2025Q3）-->
  <div class="phase-card right phase-4">
    <div class="timeline-marker" style="top:20px;"></div>
    <div class="phase-header" style="border-color: var(--phase-color-4);">
      <div class="phase-period">2025Q3</div>
      <h2 class="phase-title">平台化架构设计</h2>
    </div>

    <div class="tech-section">
      <div class="tech-label">技术规划</div>
      <div class="tech-detail">
        <p>▪ 四级协同架构：<br>
           Hardware Abstraction → RTOS Kernel → MVCS Framework → Application Layer<br>
           ▪ 模块化接口定义：<br>
            &nbsp;- &nbsp;硬件抽象规范<br>
            &nbsp;- &nbsp;平台API规划<br>
      </div>
    </div>

    <div class="tech-section">
      <div class="tech-label">开发路线</div>
      <div class="tech-detail">
        <p>▪ CI/CD流水线搭建<br>
           <span class="tech-highlight">自动化测试覆盖率≥80%</span><br>
           ▪ OTA升级框架设计<br>
           ▪ 量产准备阶段规划<br>          
      </div>
    </div>

</div>

</body>
</html>