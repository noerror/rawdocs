# Home

AI가 작성해준 문서 모음

<div class="tab">
  <button class="tablinks" onclick="openTab(event, 'Tab1')">기초</button>
  <button class="tablinks" onclick="openTab(event, 'Tab2')">실용</button>
  <button class="tablinks" onclick="openTab(event, 'Tab3')">취미</button>
</div>

<div id="Tab1" class="tabcontent">
  <h3>기초</h3>

[유니티 Json 시리얼라이즈](1/%E1%84%8B%E1%85%B2%E1%84%82%E1%85%B5%E1%84%90%E1%85%B5%20Json%20%E1%84%89%E1%85%B5%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A5%E1%86%AF%E1%84%85%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%8C%E1%85%B3%20fbfec07019fc4eb8bbc964cd4d8e04ce)

[싱글턴](1/%E1%84%89%E1%85%B5%E1%86%BC%E1%84%80%E1%85%B3%E1%86%AF%E1%84%90%E1%85%A5%E1%86%AB%20570bec3480b14d6a86fa1a636b17ac98)

[상태머신](1/%E1%84%89%E1%85%A1%E1%86%BC%E1%84%90%E1%85%A2%E1%84%86%E1%85%A5%E1%84%89%E1%85%B5%E1%86%AB%20f5b2d0584ce64d78ba1f408c379e23fe)

[C# 속성](1/C#%20%E1%84%89%E1%85%A9%E1%86%A8%E1%84%89%E1%85%A5%E1%86%BC%200edae08082bb49fba379a84e3b7789f0)

[유니티 씬 분할](1/%E1%84%8B%E1%85%B2%E1%84%82%E1%85%B5%E1%84%90%E1%85%B5%20%E1%84%8A%E1%85%B5%E1%86%AB%20%E1%84%87%E1%85%AE%E1%86%AB%E1%84%92%E1%85%A1%E1%86%AF%20efdeb390855847a78271958ff7851a7e)

[유니티 물리](1/%E1%84%8B%E1%85%B2%E1%84%82%E1%85%B5%E1%84%90%E1%85%B5%20%E1%84%86%E1%85%AE%E1%86%AF%E1%84%85%E1%85%B5%20b3dc98f7b7f2449091bbf48959ec19af)

</div>

<div id="Tab2" class="tabcontent">
  <h3>실용</h3>
  <p>This is content for Tab 2. Place any text, images, or other Markdown content here.</p>
</div>

<div id="Tab3" class="tabcontent">
  <h3>취미</h3>

[Segmentation](Segmentation)

[3DGeneration](3DGeneration)

[ImageGeneration](ImageGeneration)

[Remesh](Remesh)

[InverseRendering](InverseRendering)

[DepthEstimation](DepthEstimation)

[MachineLearning](MachineLearning)

[Rendering](Rendering)

[ImageProcessing](ImageProcessing)

[SoundProcessing](SoundProcessing)

[Motion](Motion)

[Geometry & Physics](Geometry%20&%20Physics)

</div>

<style>
/* Style the tab */
.tab {
  overflow: hidden;
  border: 1px solid #ccc;
  background-color: #f1f1f1;
}

/* Style the buttons that are used to open the tab content */
.tab button {
  background-color: inherit;
  float: left;
  border: none;
  outline: none;
  cursor: pointer;
  padding: 14px 16px;
  transition: 0.3s;
  font-size: 17px;
}

/* Change background color of buttons on hover */
.tab button:hover {
  background-color: #ddd;
}

/* Create an active/current tablink class */
.tab button.active {
  background-color: #ccc;
}

/* Style the tab content */
.tabcontent {
  display: none;
  padding: 6px 12px;
  border: 1px solid #ccc;
  border-top: none;
}
</style>

<script>
function openTab(evt, tabName) {
  var i, tabcontent, tablinks;
  tabcontent = document.getElementsByClassName("tabcontent");
  for (i = 0; i < tabcontent.length; i++) {
    tabcontent[i].style.display = "none";
  }
  tablinks = document.getElementsByClassName("tablinks");
  for (i = 0; i < tablinks.length; i++) {
    tablinks[i].className = tablinks[i].className.replace(" active", "");
  }
  document.getElementById(tabName).style.display = "block";
  evt.currentTarget.className += " active";
}
</script>