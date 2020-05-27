# PDF Ref Preview

Preview internal links in PDFs on mouse hover.

<p align="center">
  <img title="Demo" src="https://raw.githubusercontent.com/belinghy/PDFRefPreview/assets/assets/demo.gif">
</p>

A bookmarklet is a link that contains Javascript, execute it after a PDF has been loaded. To install the bookmarklet, create a bookmark and add the code below to the location field.

```js
javascript:(async()=>{const e=window.PDFViewerApplication;if("_previewHandler"in e)return document.removeEventListener("mouseover",e._previewHandler),delete e._previewHandler,void delete e._previewing;const t=await e.pdfDocument.getDestinations(),n=document.getElementById("viewer");async function i(i){if("internalLink"!=i.target.className||e._previewing)return;const r=i.target.hash,o=i.target.parentElement,a=document.createElement("canvas"),c=a.style;c.border="1px solid black",c.direction="ltr",c.position="fixed",c.zIndex="2",c.top=`${i.clientY+4}px`;const s=decodeURIComponent(r.substring(1)),d=s in t?t[s]:JSON.parse(s),p=e.pdfLinkService._cachedPageNumber(d[0]);e.pdfDocument.getPage(p).then(function(t){const n=t.getViewport({scale:1}),r=1.2*n.height*e.pdfViewer.currentScale,o=1.2*n.width*e.pdfViewer.currentScale;let s;c.height=`${r}px`,c.width=`${o}px`,c.left=`${i.clientX-o/3-4}px`;switch(d[1].name){case"XYZ":s=t.getViewport({scale:4,offsetX:4*-d[2],offsetY:4*(d[3]-n.height)});break;case"FitH":case"FitBH":case"FitV":case"FitBV":s=t.getViewport({scale:4,offsetY:4*(d[2]-n.height)});break;default:console.log(`Oops, link ${d[1].name} is not supported.`)}a.height=s.height,a.width=s.width;const p={canvasContext:a.getContext("2d"),viewport:s};t.render(p)}),n.prepend(a),e._previewing=!0,o.addEventListener("mouseleave",function(t){a.remove(),e._previewing=!1})}e._previewing=!1,document.addEventListener("mouseover",i),e._previewHandler=i})();
```

The script works by following internal links to the target page and location, and render the view in a separate canvas.

This script only works if the viewer is [PDF.js](https://github.com/mozilla/pdf.js/), which is the default in Firefox. In Chrome, it is possible to switch the PDF viewer using extensions, but this tool is not tested there.