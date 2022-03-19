# Dowloads-PDF
Code Dowloads PDF google khi chặn tải

B1 : Bạn mở tài liệu PDF cần tải trong google Drive

Bước 2 `quan trọng`: Sau đó cuộn xuống trang cuối cùng (file PDF có 10 trang thì bạn cũng phải kéo xuống trang 10 nhé), phải kéo sao cho nó load hết tất cả các trang.

Bước 3: Trên bàn phím, bạn bấm `F12` để mở Developer Tools, sau đó click vào tab `Console`

Bước 4: `Sao chép` mã Code dưới đây và dán vào tab Console

```
let jspdf = document.createElement("script");
jspdf.onload = function () {
    let pdfDocumentName = "AikoCuteHotMe";
    let doc;
    function generatePDF (){
        let imgTags = document.getElementsByTagName("img");
        let checkURLString = "blob:https://drive.google.com/";
        let validImgTagCounter = 0;
        for (i = 0; i < imgTags.length; i++) {
            if (imgTags[i].src.substring(0, checkURLString.length) === checkURLString){
                validImgTagCounter = validImgTagCounter + 1;
                //console.log(imgTags[i].src);
                let img = imgTags[i];
                let canvas = document.createElement('canvas');
                let context = canvas.getContext("2d");
                canvas.width = img.naturalWidth;
                canvas.height = img.naturalHeight;
                //console.log("Width: " + img.naturalWidth + ", Height: " + img.naturalHeight);
                context.drawImage(img, 0, 0, img.naturalWidth, img.naturalHeight);
                let imgDataURL = canvas.toDataURL();
               // console.log(imgDataURL);
                //let ratio;
                let orientation;
                if (img.naturalWidth > img.naturalHeight){
                    //console.log("Landscape");
                    orientation = "l";
                    //ratio = img.naturalWidth/img.naturalHeight
                }else {
                    //console.log("Portrait");
                    orientation = "p";
                    //ratio = img.naturalWidth/img.naturalHeight
                }
                let scalefactor = 1.335;
                let pageWidth = img.naturalWidth * scalefactor;
                let pageHeight = img.naturalHeight * scalefactor;
               //let imagexLeft = (pageWidth - img.naturalWidth)/2;
               //let imagexTop = (pageHeight - img.naturalHeight)/2;
                if (validImgTagCounter === 1){
                    doc = new jsPDF({
                        orientation: orientation,
                        unit: "px",
                        format: [pageWidth, pageHeight],
                    });
                    doc.addImage(imgDataURL, "PNG", 0, 0, img.naturalWidth, img.naturalHeight);
                }else{
                    doc.addPage([pageWidth, pageHeight] , orientation);
                    doc.addImage(imgDataURL, "PNG", 0, 0, img.naturalWidth, img.naturalHeight);
                }
            }
        }
        pdfDocumentName = pdfDocumentName + ".pdf";
       doc.save(pdfDocumentName);
    }
    let allElements = document.querySelectorAll("*");
    let chosenElement;
    let heightOfScrollableElement = 0;
    for (i = 0; i < allElements.length; i++) {
        if ( allElements[i].scrollHeight>=allElements[i].clientHeight){
            if (heightOfScrollableElement < allElements[i].scrollHeight){
                //console.log(allElements[i]);
                //console.log(allElements[i].scrollHeight);
                heightOfScrollableElement = allElements[i].scrollHeight;
                chosenElement = allElements[i];
            }
        }
    }
    if (chosenElement.scrollHeight > chosenElement.clientHeight){
        console.log("Auto Scroll");
        let scrollDistance = Math.round(chosenElement.clientHeight/2);
        //console.log("scrollHeight: " + chosenElement.scrollHeight);
        //console.log("scrollDistance: " + scrollDistance);
        //www.gdrive.vip
        let loopCounter = 0;
        function myLoop(remainingHeightToScroll, scrollToLocation) {
            loopCounter = loopCounter+1;
            console.log(loopCounter);
            setTimeout(function() {
                if (remainingHeightToScroll === 0){
                    scrollToLocation = scrollDistance;
                    chosenElement.scrollTo(0, scrollToLocation);
                    remainingHeightToScroll = chosenElement.scrollHeight - scrollDistance;
                }else{
                    scrollToLocation = scrollToLocation + scrollDistance ;
                    chosenElement.scrollTo(0, scrollToLocation);
                    remainingHeightToScroll = remainingHeightToScroll - scrollDistance;
                }
                if (remainingHeightToScroll >= chosenElement.clientHeight){
                    myLoop(remainingHeightToScroll, scrollToLocation)
                }else{
                    setTimeout(function() {
                        generatePDF();
                    }, 1500)
                }
            }, 500)
        }
        myLoop(0, 0);
    }else{
        console.log("No Scroll");
        setTimeout(function() {
            generatePDF();
        }, 1500)
    }
};
jspdf.src = 'https://gdrive.vip/wp-content/uploads/2020/jspdf.debug.js';
document.body.appendChild(jspdf);
```
Bước 5: Nhấn Enter và đợi kết quả :))). Nhanh hay chậm tùy thuộc vào số trang trong file pdf đó, nên đừng nóng vội nhé . 