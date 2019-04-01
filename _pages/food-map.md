---
layout: default
permalink: /food-map/
---

<style type="text/css">
.info_window {
    margin: 10px;
}
</style>
<script type="text/javascript" src="https://openapi.map.naver.com/openapi/v3/maps.js?ncpClientId=ia9wjc5v1n"></script>
<div id="map" style="width:100%;height:800px;"></div>
<script>

// (o) 상호명
// (o) 주소
// (수집) 방영 차수/방영일
// (수집) 대표 메뉴
// (수집) 클립영상 링크
// 길찾기

window.addEventListener('DOMContentLoaded', function () {
    $.ajax({url: '/assets/json/food_temp.json', success: function(dataArray) {
        console.dir(dataArray);
        var latLngArray = dataArray.map(data => [data, new naver.maps.LatLng(data.py, data.px)]);
        var markerArray = latLngArray.map(arr => new naver.maps.Marker({
            map: map,
            position: arr[1],
            data: arr[0], // custom property
            infoWindow: getInfoWindow(arr[0])
        }));
        markerArray.forEach(marker => {
            naver.maps.Event.addListener(marker, "click", function(e) {
                var infowindow = marker.infoWindow;
                if (infowindow.getMap()) {
                    infowindow.close();
                } else {
                    infowindow.open(map, marker);
                }
            });
        });
    }});
})
var map = new naver.maps.Map('map', {
        //center: cityhall,
        zoom: 4
    });

function getContentString(data) {
    return [
        '<div class="info_window">',
        '   <h3>' + data.name + '</h3>',
        '   <p>' + data.address,
        '   </p>',
        '</div>'
    ].join('');
}

function getInfoWindow(data) {
    return new naver.maps.InfoWindow({
        content: getContentString(data),
        maxWidth: 280,
        backgroundColor: "#ffffff",
        borderColor: "#808080",
        borderWidth: 1,
        anchorSize: new naver.maps.Size(30, 30),
        anchorSkew: true,
        anchorColor: "#ffffff",
        pixelOffset: new naver.maps.Point(20, -20)
    });
}
</script>