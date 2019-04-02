---
layout: default
permalink: /food-map/
---

<style type="text/css">
.info_window {
    margin: 10px;
}
.btn_mylct {
    border: 1px solid rgba(58,70,88,.45);
    border-radius: 20px;
    background: #fcfcfd;
    color: transparent;
    display: block;
    margin: 5px;
}
.location {
    background: url("/assets/images/location.png");
    background-repeat: no-repeat;
    background-size: 25px 25px;
    background-position-x: 2px;
    background-position-y: 5px;
    width:32px;
    height:32px;
    display: -webkit-box;
}
.red_dot {
    width: 18px;
    height: 18px;
    background-color: red;
    border-radius: 50%;
    border: 2px solid white;
    display: block;
}
</style>
<script type="text/javascript" src="https://openapi.map.naver.com/openapi/v3/maps.js?ncpClientId=ia9wjc5v1n"></script>
<div id="map" style="width:100%;height:550px;"></div>
<div>Icons made by <a href="https://www.flaticon.com/authors/hadrien" title="Hadrien">Hadrien</a> from <a href="https://www.flaticon.com/"              title="Flaticon">www.flaticon.com</a> is licensed by <a href="http://creativecommons.org/licenses/by/3.0/"              title="Creative Commons BY 3.0" target="_blank">CC 3.0 BY</a></div>
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

var customControl = new naver.maps.CustomControl('<a href="#" class="btn_mylct"><span class="location"></span></a>', {
    position: naver.maps.Position.TOP_RIGHT
});
customControl.setMap(map);

var userLocationMarker = getUserLocationMarker(map);
var domEventListener = (function (map, customControl, userLocationMarker) {
    return naver.maps.Event.addDOMListener(customControl.getElement(), 'click', function() {
        navigator.geolocation.getCurrentPosition(
            function (position) {
                var latitude = position.coords.latitude;
                var longitude = position.coords.longitude;
                var latLng = new naver.maps.LatLng(latitude, longitude)
                map.setCenter(latLng);

                userLocationMarker.setPosition(latLng);
                userLocationMarker.setVisible(true);
            },
            function (error) {
                console.error(error);
            }
        );
    });
})(map, customControl, userLocationMarker);

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

function getUserLocationMarker(map) {
    return new naver.maps.Marker({
        map: map,
        icon: {
            content: '<span class="red_dot"></span>'
        },
        visible: false
    });
}
</script>