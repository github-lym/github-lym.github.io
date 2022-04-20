# object-fit—圖片的縮放


發現圖片有新屬性，記錄一下。    
<!--more-->

<div>
  <h3>Original 原圖</h3>
  <img src="/images/devil_s_bg.png" alt="devil"/>

  <h3>object-fit: none 圖片不做縮放但還是限制在框框的範圍</h3>
  <img src="/images/devil_s_bg.png" alt="devil" style="width: 150px; height: 100px;object-fit: none"/>

  <h3>object-fit: fill 粗暴填滿</h3>
  <img src="/images/devil_s_bg.png" alt="devil" style="width: 150px; height: 100px;object-fit: fill"/>

  <h3>object-fit: contain 等比例縮放</h3>
  <img src="/images/devil_s_bg.png" alt="devil" style="width: 150px; height: 100px;object-fit: contain"/>

  <h3>object-fit: cover 儘可能佔滿容器為原則，超出容器部份會被截掉</h3>
  <img src="/images/devil_s_bg.png" alt="devil" style="width: 150px; height: 100px;object-fit: cover"/>

  <h3>object-fit: scale-down 只等比例縮小不放大</h3>
  <img src="/images/devil_s_bg.png" alt="devil" style="width: 150px; height: 100px;object-fit: scale-down"//>
</div>

