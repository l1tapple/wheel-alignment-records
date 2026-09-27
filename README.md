# wheel-alignment-records
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>四轮定位数据数据库</title>
  <link rel="stylesheet" href="styles.css" />
</head>
<body>
  <!-- ============ 首页视图 ============ -->
  <div id="homeView">
    <header class="navbar">
      <div class="navbar-inner navbar-flex">
        <div class="logo">
          <span class="logo-icon">📍</span>
          <span class="logo-text">四轮定位数据数据库</span>
        </div>
        <div class="live-clock" title="实时北京时间">
          <span class="clock-icon">🕐</span>
          <span id="liveClock">--</span>
        </div>
      </div>
    </header>

    <main class="container">
      <!-- 搜索 + 筛选卡片 -->
      <section class="search-card">
        <div class="search-box">
          <svg class="search-icon" viewBox="0 0 24 24" width="20" height="20" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <circle cx="11" cy="11" r="8"></circle>
            <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
          </svg>
          <input id="searchInput" type="text" placeholder="搜索车牌、修理厂、备注、金额、日期…" autocomplete="off" />
          <button id="clearSearchBtn" class="clear-btn" type="button" title="清空" hidden>&times;</button>
        </div>
        <!-- 筛选标签 -->
        <div class="filter-bar">
          <div class="filter-group">
            <span class="filter-label">车牌</span>
            <div class="filter-chips" id="plateChips">
              <button type="button" class="filter-chip active" data-filter="plateType" data-value="全部">全部</button>
              <button type="button" class="filter-chip" data-filter="plateType" data-value="无牌">无牌</button>
              <button type="button" class="filter-chip" data-filter="plateType" data-value="临牌">临牌</button>
              <button type="button" class="filter-chip" data-filter="plateType" data-value="蓝牌">蓝牌</button>
              <button type="button" class="filter-chip" data-filter="plateType" data-value="绿牌">绿牌</button>
            </div>
          </div>
          <div class="filter-divider"></div>
          <div class="filter-group">
            <span class="filter-label">客户</span>
            <div class="filter-chips" id="custChips">
              <button type="button" class="filter-chip active" data-filter="customerType" data-value="全部">全部</button>
              <button type="button" class="filter-chip" data-filter="customerType" data-value="个人">个人</button>
              <button type="button" class="filter-chip" data-filter="customerType" data-value="修理厂">修理厂</button>
            </div>
          </div>
          <div class="filter-divider"></div>
          <div class="filter-group">
            <span class="filter-label">时间</span>
            <div class="filter-chips" id="timeChips">
              <button type="button" class="filter-chip active" data-time="全部">全部</button>
              <button type="button" class="filter-chip" data-time="今天">今天</button>
              <button type="button" class="filter-chip" data-time="近7天">近7天</button>
              <button type="button" class="filter-chip" data-time="近30天">近30天</button>
            </div>
            <input id="dateFrom" type="date" class="date-input" title="开始日期" />
            <span class="date-sep">至</span>
            <input id="dateTo" type="date" class="date-input" title="结束日期" />
            <button id="clearDateBtn" type="button" class="filter-chip" hidden>清除日期</button>
          </div>
        </div>
      </section>

      <!-- 数据统计 + 新建按钮 -->
      <section class="stats-row">
        <div class="stats-cards">
          <div class="stat-card">
            <span class="stat-num" id="statTotal">0</span>
            <span class="stat-label">总记录</span>
          </div>
          <div class="stat-card">
            <span class="stat-num" id="statToday">0</span>
            <span class="stat-label">今日新增</span>
          </div>
          <div class="stat-card">
            <span class="stat-num" id="statAmount">¥0</span>
            <span class="stat-label">累计金额</span>
          </div>
        </div>
        <button id="newDocBtn" class="btn-primary" type="button">
          <span class="btn-plus">＋</span> 新建文档
        </button>
      </section>

      <!-- 最新文档展示 -->
      <section class="docs-section">
        <div class="section-header">
          <h2 id="sectionTitle">最新文档</h2>
          <span id="docCount" class="doc-count"></span>
        </div>
        <div id="docsGrid" class="docs-grid"></div>
        <div id="emptyState" class="empty-state" hidden>
          <div class="empty-icon">📄</div>
          <p id="emptyText">还没有文档，点击「新建文档」创建第一条记录吧</p>
        </div>
      </section>
    </main>
  </div>

  <!-- ============ 新建文档视图 ============ -->
  <div id="createView" hidden>
    <header class="navbar">
      <div class="navbar-inner navbar-flex">
        <button id="backBtn" class="btn-back" type="button">‹ 返回</button>
        <span class="navbar-title">新建文档</span>
        <span class="navbar-placeholder"></span>
      </div>
    </header>

    <main class="container">
      <form id="createForm">
        <!-- 1. 车牌类型 -->
        <section class="form-card">
          <div class="form-card-title">车牌类型</div>
          <div class="plate-type-grid">
            <label class="plate-type-item">
              <input type="radio" name="plateType" value="无牌" checked />
              <span class="type-chip chip-none">无牌</span>
            </label>
            <label class="plate-type-item">
              <input type="radio" name="plateType" value="临牌" />
              <span class="type-chip chip-temp">临牌</span>
            </label>
            <label class="plate-type-item">
              <input type="radio" name="plateType" value="蓝牌" />
              <span class="type-chip chip-blue">蓝牌</span>
            </label>
            <label class="plate-type-item">
              <input type="radio" name="plateType" value="绿牌" />
              <span class="type-chip chip-green">绿牌</span>
            </label>
          </div>
        </section>

        <!-- 2. 车牌输入（选择非无牌时显示） -->
        <section class="form-card" id="plateSection" hidden>
          <div class="form-card-title">车牌号码 <span class="required">*</span></div>
          <div id="platePreview" class="plate-preview plate-empty" role="button" title="点击输入车牌"></div>
          <p class="field-hint" id="plateHint"></p>
        </section>

        <!-- 3. 图片上传 -->
        <section class="form-card">
          <div class="form-card-title">上传图片 <span class="title-sub">（可从相册选择，数量不限）</span></div>
          <div class="upload-wrap">
            <div id="imageList" class="upload-grid"></div>
            <label class="upload-add">
              <input id="imageInput" type="file" accept="image/*" multiple hidden />
              <span class="upload-add-icon">📷</span>
              <span class="upload-add-text">添加图片</span>
            </label>
          </div>
        </section>

        <!-- 4. 客户类型 + 修理厂 -->
        <section class="form-card">
          <div class="form-card-title">客户类型</div>
          <div class="select-row">
            <select id="customerType" class="select-field">
              <option value="个人">个人</option>
              <option value="修理厂">修理厂</option>
            </select>
          </div>
          <div id="shopRow" class="select-row" hidden>
            <select id="shopSelect" class="select-field"></select>
            <button type="button" id="addShopBtn" class="btn-ghost btn-sm">＋ 新增</button>
          </div>
        </section>

        <!-- 5. 金额 -->
        <section class="form-card">
          <div class="form-card-title">金额</div>
          <div class="price-row">
            <span id="priceTag" class="price-tag">80 元</span>
            <div class="price-edit">
              <input id="priceInput" type="number" min="0" step="1" value="80" />
              <span class="price-unit">元</span>
            </div>
          </div>
        </section>

        <!-- 6. 备注 -->
        <section class="form-card">
          <div class="form-card-title">备注</div>
          <textarea id="remarkInput" rows="3" placeholder="添加备注（选填）…"></textarea>
        </section>

        <button type="submit" class="btn-primary btn-block" id="saveDocBtn">保存文档</button>
      </form>
    </main>
  </div>

  <!-- ============ 文档详情视图 ============ -->
  <div id="detailView" hidden>
    <header class="navbar">
      <div class="navbar-inner navbar-flex">
        <button id="detailBackBtn" class="btn-back" type="button">‹ 返回</button>
        <span class="navbar-title">文档详情</span>
        <span class="navbar-placeholder"></span>
      </div>
    </header>

    <main class="container">
      <!-- 车牌大图 -->
      <section class="form-card detail-plate-card">
        <div id="detailPlate" class="detail-plate"></div>
        <div id="detailTime" class="detail-time"></div>
      </section>

      <!-- 信息列表 -->
      <section class="form-card">
        <div class="info-row">
          <span class="info-label">车牌类型</span>
          <span class="info-value" id="detailPlateType"></span>
        </div>
        <div class="info-row">
          <span class="info-label">客户类型</span>
          <span class="info-value" id="detailCustomerType"></span>
        </div>
        <div class="info-row" id="detailShopRow" hidden>
          <span class="info-label">修理厂</span>
          <span class="info-value" id="detailShop"></span>
        </div>
        <div class="info-row">
          <span class="info-label">金额</span>
          <span class="info-value price-highlight" id="detailPrice"></span>
        </div>
        <div class="info-row" id="detailRemarkRow" hidden>
          <span class="info-label">备注</span>
          <span class="info-value info-remark" id="detailRemark"></span>
        </div>
      </section>

      <!-- 图片 -->
      <section class="form-card" id="detailImagesCard" hidden>
        <div class="form-card-title">图片 <span class="title-sub" id="detailImageCount"></span></div>
        <div id="detailImageGrid" class="detail-image-grid"></div>
      </section>

      <button id="detailDeleteBtn" class="btn-danger-block" type="button">删除该记录</button>
    </main>
  </div>

  <!-- ============ 删除确认弹窗 ============ -->
  <div id="deleteModal" class="modal-overlay" hidden>
    <div class="modal delete-modal">
      <div class="delete-icon-wrap">⚠️</div>
      <h3 class="delete-title">确认删除这条记录？</h3>
      <p class="delete-desc" id="deleteDesc"></p>
      <p class="delete-warn">删除后无法恢复，请再次确认！</p>
      <div class="delete-btns">
        <button id="deleteCancelBtn" type="button" class="btn-ghost">取消</button>
        <button id="deleteConfirmBtn" type="button" class="btn-danger">确认删除</button>
      </div>
    </div>
  </div>

  <!-- ============ 图片灯箱 ============ -->
  <div id="lightbox" class="lightbox" hidden>
    <button id="lbClose" class="lb-close" type="button">&times;</button>
    <button id="lbPrev" class="lb-nav lb-prev" type="button">‹</button>
    <img id="lbImg" alt="" />
    <button id="lbNext" class="lb-nav lb-next" type="button">›</button>
    <div id="lbCounter" class="lb-counter"></div>
  </div>

  <!-- ============ 车牌键盘弹层 ============ -->
  <div id="plateModal" class="plate-modal" hidden>
    <div class="plate-modal-mask" id="plateMask"></div>
    <div class="plate-keyboard">
      <div class="kb-header">
        <span id="kbTitle" class="kb-title">请输入车牌</span>
        <button id="kbClose" class="kb-close" type="button">&times;</button>
      </div>
      <div id="kbPlate" class="kb-plate"></div>
      <p id="kbHint" class="kb-hint"></p>
      <div id="kbBody" class="kb-body"></div>
      <div class="kb-footer">
        <button id="kbDelete" class="kb-key kb-key-func" type="button">删除</button>
        <button id="kbOk" class="kb-key kb-key-ok" type="button">确定</button>
      </div>
    </div>
  </div>

  <!-- ============ 新增修理厂弹窗 ============ -->
  <div id="shopModal" class="modal-overlay" hidden>
    <div class="modal">
      <div class="modal-header">
        <h3>新增修理厂</h3>
        <button id="shopModalClose" class="modal-close" type="button">&times;</button>
      </div>
      <form id="shopForm">
        <div class="form-group">
          <label for="shopNameInput">修理厂名称 <span class="required">*</span></label>
          <input id="shopNameInput" type="text" placeholder="例如：顺达汽车修理厂" maxlength="30" required />
        </div>
        <div class="modal-footer">
          <button type="button" id="shopCancelBtn" class="btn-ghost">取消</button>
          <button type="submit" class="btn-primary">保存</button>
        </div>
      </form>
    </div>
  </div>

  <div id="toast" class="toast" hidden></div>

  <script src="app.js"></script>
</body>
</html>
