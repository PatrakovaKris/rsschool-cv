1. Khabibullina Kristina
2. Mobile phone number:+7(982)9914916, e-mail: khabibullinakris@gmail.com
3. I want to be a first-class programmer and create high-quality designed sites that will make the world a little more beautiful. My goal is to get a job at Google. The most important thing for me is the constant development of my skills.
4. Skills:
    * Programming Languages: HTML, CSS, JS/JQuery, PHP, SQL.
    * Frameworks: basic knowledge of React.js.
    * Version control: Github.
    * Tools: Apache HTTP Server, YandexAPI, SASS, LESS.
5. Code examples:
~~~ 
sapedit.map = {
    init: function () {
        this.myMap = new ymaps.Map('map', {
            center: [56.852775, 53.211463],
            zoom: 7,
            controls: [],
            type: 'yandex#hybrid'
        }),
              // Создаем экземпляр класса ymaps.control.SearchControl
        this.mySearchControl = new ymaps.control.SearchControl({
            options: {
                searchControlProvider: 'yandex#map',
                noPlacemark: true ,
                suppressYandexSearch: true
            }
        });

        this.mySearchResults = new ymaps.GeoObjectCollection(null, {
                hintContentLayout: ymaps.templateLayoutFactory.createClass('$[properties.name]')
            });

        this.myMap.controls.add(this.mySearchControl);
        this.myMap.geoObjects.add(this.mySearchResults);

          this.mySearchResults.events.add('click', function (e) {
            e.get('target').options.set('preset', 'islands#redIcon');
        });

        this.mySearchControl.events.add('resultselect', function (e) {
            var index = e.get('index');

            self.mySearchControl.getResult(index).then(function (res) {
                // self.mySearchResults.add(res);
                // console.log(res);
            });
        }).add('submit', function () {
            self.mySearchResults.removeAll();
        });

        this.sapCollection = new ymaps.GeoObjectCollection();

        this.myMap.controls.add('zoomControl', {
            float: 'none',
            position: {
                left: 10,
                top: 100
            }
        });

        this.myMap.controls.add('rulerControl', {
            float: 'none',
            scaleLine: 'none',
            size: 'large',
            position: {
                left: 100,
                top: 50
            }
        });

        this.myMap.controls.add('typeSelector', {
            float: 'none',
            position: {
                left: 10,
                top: 50
            }
        });

        this.myMap.controls.add('geolocationControl', {
            float: 'none',
            position: {
                left: 10,
                top: 10
            }
        });
        var cursor = this.myMap.cursors.push('arrow');
        var self = this;
        $(window).on('placeFromSlider', function (e, point) {
            self.placeFromSlider(point);
        });
    },

    objectsToArray: function(){
        self = this;
        var pylons = [];
        self.sapCollection.each(function (object){
            var pylon = {
              name : object.properties._data.iconContent,
           objname : object.properties._data.hintContent,
                 x : object.geometry._coordinates[0],
                 y : object.geometry._coordinates[1]
            };
            pylons.push(pylon);
        });
        sapedit.mainMenu.putDataEx(pylons);
    },

    showMarkVL: function(data){
        this.sapCollection.removeAll();
        if (!data) return false;
         var key1 = 'Код опоры';
         var key2 = 'Код участка';
         var key3 = "Координата X ";
         var key4 = 'Координата Y';
         var key5 = 'Наименование опоры';
            for(i = 1; i < data.length; i++) {
                var coords = [data[i][key3], data[i][key4]];
                var point = new ymaps.GeoObject({
                    // Описание геометрии.
                    geometry: {
                        type: "Point",
                        coordinates: coords
                    },
                    // Свойства.
                    properties: {
                        // Контент метки.
                        iconContent: data[i][key5],
                        hintContent: data[i][key5]
                    }
                }, {
                    draggable: true,
                    preset: "islands#redStretchyIcon",
                });
                this.sapCollection.add(point);
            };
            this.myMap.geoObjects.add(this.sapCollection);
            this.myMap.setBounds(this.sapCollection.getBounds(), {checkZoomRange: true})
    },

    showMarkKL: function(data){
        this.sapCollection.removeAll();
        if (!data) return false;
        var key1 = '__EMPTY_3';
        var key2 = '__EMPTY_11';
        var key3 = '__EMPTY_17';
            for (i = 5; i < data.length; i++) {
                if (data[i][key2].indexOf('Широта точка') + 1) {
                    var numX = Number(data[i][key2].replace(/\D+/g, ""));
                    for (j = 5; j < data.length; j++) {
                        if (data[i][key1] == data[j][key1] && data[j][key2].indexOf('Долгота точка') + 1 && numX == Number(data[j][key2].replace(/\D+/g, "")) && data[j][key3]) {
                            var coords = [data[i][key3], data[j][key3]];
                            var point = new ymaps.GeoObject({
                                    geometry: {
                                        type: "Point",
                                        coordinates: coords
                                    },
                                    properties: {
                                        // Контент метки.
                                        iconContent: 'Точка ' + numX,
                                        hintContent: data[i][key1]
                                    }
                                },
                                {
                                    draggable: true,
                                    preset: "islands#redStretchyIcon",
                                }
                            );
                            this.sapCollection.add(point);
                        }
                    }
                }
            }
            this.myMap.geoObjects.add(this.sapCollection);
            this.myMap.setBounds(this.sapCollection.getBounds(), {checkZoomRange: true})
    },

    showMarkTP: function(data){

        this.sapCollection.removeAll();

        if (!data) return false;

        var key1 = 'Наименование КО';
        var key2 = '__EMPTY_17';

            for (i = 5; i < data.length - 1; i++) {
                if (data[i][key1] == data[i + 1][key1]) {

                    var coords = [data[i][key2], data[i + 1][key2]];

                    var point = new ymaps.GeoObject({
                        // Описание геометрии.
                        geometry: {
                            type: "Point",
                            coordinates: coords
                        },
                        // Свойства.
                        properties: {
                            // Контент метки.
                            iconContent: data[i][key1],
                            hintContent: data[i][key1]
                        }
                    }, {
                        draggable: true,
                        preset: "islands#redStretchyIcon",
                    });
                    this.sapCollection.add(point);
                }
            };

            this.myMap.geoObjects.add(this.sapCollection);
            this.myMap.setBounds(this.sapCollection.getBounds(), {checkZoomRange: true})
    },
    };
~~~
6. Experience: 1 year frontend developer.
7. Education: Udmurt State University master's degree in fundamental Informatics and information technology (honors degree)
