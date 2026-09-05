# MapLibre GL JS Official API Reference Catalog 📖🗺️

> Exhaustive index of all classes, interfaces, global functions, and type definitions from the official **MapLibre GL JS v6** documentation at [`maplibre.org/maplibre-gl-js/docs/API/`](https://maplibre.org/maplibre-gl-js/docs/API/).

Every entity links directly to the live official MapLibre documentation to enable immediate API lookups.

---

## 📑 Architectural Domain Index

* [**Core Classes & Viewport**](#1-core-classes--viewport) (60 classes)
* [**Interfaces & Options**](#2-interfaces--options) (15 interfaces)
* [**Global Functions**](#3-global-functions) (21 functions)
* [**Type Aliases & Enums**](#4-type-aliases--enums) (104 types)

---

## 1. Core Classes & Viewport

| Class | Category / Description | Official Documentation Link |
| :--- | :--- | :--- |
| **`maplibregl.AJAXError`** | `Core Class` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/AJAXError/) |
| **`maplibregl.AttributionControl`** | `Control` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/AttributionControl/) |
| **`maplibregl.BoxZoomHandler`** | `Handler` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/BoxZoomHandler/) |
| **`maplibregl.CanvasSource`** | `Source` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/CanvasSource/) |
| **`maplibregl.CooperativeGesturesHandler`** | `Handler` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/CooperativeGesturesHandler/) |
| **`maplibregl.DoubleClickZoomHandler`** | `Handler` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/DoubleClickZoomHandler/) |
| **`maplibregl.DragPanHandler`** | `Handler` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/DragPanHandler/) |
| **`maplibregl.DragRotateHandler`** | `Handler` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/DragRotateHandler/) |
| **`maplibregl.EdgeInsets`** | `Core Class` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/EdgeInsets/) |
| **`maplibregl.ErrorEvent`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/ErrorEvent/) |
| **`maplibregl.Event<TType extends string = string>`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/Event/) |
| **`maplibregl.Evented<EventType extends EventTypeMap = EventTypeMap>`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/Evented/) |
| **`maplibregl.FullscreenControl`** | `Control` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/FullscreenControl/) |
| **`maplibregl.FullscreenEvent`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/FullscreenEvent/) |
| **`maplibregl.GPUInitializationError`** | `Core Class` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/GPUInitializationError/) |
| **`maplibregl.GeoJSONSource`** | `Source` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/GeoJSONSource/) |
| **`maplibregl.GeolocateControl`** | `Control` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/GeolocateControl/) |
| **`maplibregl.GeolocateErrorEvent`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/GeolocateErrorEvent/) |
| **`maplibregl.GeolocateEvent`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/GeolocateEvent/) |
| **`maplibregl.GeolocatePositionEvent`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/GeolocatePositionEvent/) |
| **`maplibregl.GlobeControl`** | `Control` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/GlobeControl/) |
| **`maplibregl.Hash`** | `Core Class` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/Hash/) |
| **`maplibregl.ImageSource`** | `Source` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/ImageSource/) |
| **`maplibregl.KeyboardHandler`** | `Handler` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/KeyboardHandler/) |
| **`maplibregl.LngLat`** | `Core Class` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/LngLat/) |
| **`maplibregl.LngLatBounds`** | `Core Class` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/LngLatBounds/) |
| **`maplibregl.LogoControl`** | `Control` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/LogoControl/) |
| **`maplibregl.Map`** | `Core Class` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/Map/) |
| **`maplibregl.MapBoxZoomEvent`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/MapBoxZoomEvent/) |
| **`maplibregl.MapContextEvent`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/MapContextEvent/) |
| **`maplibregl.MapLibreEvent<TOrig = unknown>`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/MapLibreEvent/) |
| **`maplibregl.MapMouseEvent`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/MapMouseEvent/) |
| **`maplibregl.MapMovementEvent`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/MapMovementEvent/) |
| **`maplibregl.MapProjectionEvent`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/MapProjectionEvent/) |
| **`maplibregl.MapSourceDataEvent`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/MapSourceDataEvent/) |
| **`maplibregl.MapStyleDataEvent`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/MapStyleDataEvent/) |
| **`maplibregl.MapStyleImageMissingEvent`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/MapStyleImageMissingEvent/) |
| **`maplibregl.MapStyleLoadEvent`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/MapStyleLoadEvent/) |
| **`maplibregl.MapTerrainEvent`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/MapTerrainEvent/) |
| **`maplibregl.MapTouchEvent`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/MapTouchEvent/) |
| **`maplibregl.MapWheelEvent`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/MapWheelEvent/) |
| **`maplibregl.Marker`** | `Core Class` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/Marker/) |
| **`maplibregl.MarkerClickEvent`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/MarkerClickEvent/) |
| **`maplibregl.MarkerDragEvent`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/MarkerDragEvent/) |
| **`maplibregl.MercatorCoordinate`** | `Core Class` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/MercatorCoordinate/) |
| **`maplibregl.NavigationControl`** | `Control` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/NavigationControl/) |
| **`maplibregl.Popup`** | `Core Class` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/Popup/) |
| **`maplibregl.PopupEvent`** | `Event` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/PopupEvent/) |
| **`maplibregl.RasterDEMTileSource`** | `Source` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/RasterDEMTileSource/) |
| **`maplibregl.RasterTileSource`** | `Source` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/RasterTileSource/) |
| **`maplibregl.ScaleControl`** | `Control` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/ScaleControl/) |
| **`maplibregl.ScrollZoomHandler`** | `Handler` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/ScrollZoomHandler/) |
| **`maplibregl.Style`** | `Core Class` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/Style/) |
| **`maplibregl.TerrainControl`** | `Control` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/TerrainControl/) |
| **`maplibregl.TwoFingersTouchPitchHandler`** | `Handler` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/TwoFingersTouchPitchHandler/) |
| **`maplibregl.TwoFingersTouchRotateHandler`** | `Handler` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/TwoFingersTouchRotateHandler/) |
| **`maplibregl.TwoFingersTouchZoomHandler`** | `Handler` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/TwoFingersTouchZoomHandler/) |
| **`maplibregl.TwoFingersTouchZoomRotateHandler`** | `Handler` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/TwoFingersTouchZoomRotateHandler/) |
| **`maplibregl.VectorTileSource`** | `Source` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/VectorTileSource/) |
| **`maplibregl.VideoSource`** | `Source` | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/classes/VideoSource/) |

---

## 2. Interfaces & Options

| Interface | Description | Official Documentation Link |
| :--- | :--- | :--- |
| **`Actor`** | Specification Interface | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/interfaces/Actor/) |
| **`AlphaImage`** | Specification Interface | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/interfaces/AlphaImage/) |
| **`CustomLayerInterface`** | Specification Interface | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/interfaces/CustomLayerInterface/) |
| **`Dispatcher`** | Specification Interface | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/interfaces/Dispatcher/) |
| **`FeatureIndex`** | Specification Interface | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/interfaces/FeatureIndex/) |
| **`GeoJSONFeature`** | Specification Interface | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/interfaces/GeoJSONFeature/) |
| **`Handler`** | Specification Interface | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/interfaces/Handler/) |
| **`IActor`** | Specification Interface | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/interfaces/IActor/) |
| **`IControl`** | Specification Interface | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/interfaces/IControl/) |
| **`OverscaledTileID`** | Specification Interface | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/interfaces/OverscaledTileID/) |
| **`Source`** | Specification Interface | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/interfaces/Source/) |
| **`StyleImageInterface`** | Specification Interface | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/interfaces/StyleImageInterface/) |
| **`StyleLayer`** | Specification Interface | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/interfaces/StyleLayer/) |
| **`Subscription`** | Specification Interface | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/interfaces/Subscription/) |
| **`Tile`** | Specification Interface | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/interfaces/Tile/) |

---

## 3. Global Functions

| Function | Description | Official Documentation Link |
| :--- | :--- | :--- |
| **`maplibregl.addProtocol()()`** | Top-level runtime utility | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/functions/addProtocol/) |
| **`maplibregl.addSourceType()()`** | Top-level runtime utility | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/functions/addSourceType/) |
| **`maplibregl.clearPrewarmedResources()()`** | Top-level runtime utility | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/functions/clearPrewarmedResources/) |
| **`maplibregl.createTileMesh()()`** | Top-level runtime utility | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/functions/createTileMesh/) |
| **`maplibregl.getGlobalDispatcher()()`** | Top-level runtime utility | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/functions/getGlobalDispatcher/) |
| **`maplibregl.getMaxParallelImageRequests()()`** | Top-level runtime utility | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/functions/getMaxParallelImageRequests/) |
| **`maplibregl.getRTLTextPluginStatus()()`** | Top-level runtime utility | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/functions/getRTLTextPluginStatus/) |
| **`maplibregl.getVersion()()`** | Top-level runtime utility | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/functions/getVersion/) |
| **`maplibregl.getWorkerCount()()`** | Top-level runtime utility | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/functions/getWorkerCount/) |
| **`maplibregl.getWorkerUrl()()`** | Top-level runtime utility | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/functions/getWorkerUrl/) |
| **`maplibregl.importScriptInWorkers()()`** | Top-level runtime utility | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/functions/importScriptInWorkers/) |
| **`maplibregl.isTimeFrozen()()`** | Top-level runtime utility | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/functions/isTimeFrozen/) |
| **`maplibregl.now()()`** | Top-level runtime utility | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/functions/now/) |
| **`maplibregl.prewarm()()`** | Top-level runtime utility | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/functions/prewarm/) |
| **`maplibregl.removeProtocol()()`** | Top-level runtime utility | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/functions/removeProtocol/) |
| **`maplibregl.restoreNow()()`** | Top-level runtime utility | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/functions/restoreNow/) |
| **`maplibregl.setMaxParallelImageRequests()()`** | Top-level runtime utility | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/functions/setMaxParallelImageRequests/) |
| **`maplibregl.setNow()()`** | Top-level runtime utility | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/functions/setNow/) |
| **`maplibregl.setRTLTextPlugin()()`** | Top-level runtime utility | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/functions/setRTLTextPlugin/) |
| **`maplibregl.setWorkerCount()()`** | Top-level runtime utility | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/functions/setWorkerCount/) |
| **`maplibregl.setWorkerUrl()()`** | Top-level runtime utility | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/functions/setWorkerUrl/) |

---

## 4. Type Aliases & Enums

| Type / Definition | Official Documentation Link |
| :--- | :--- |
| **`ActorMessage<T extends MessageType>`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/ActorMessage/) |
| **`AddLayerObject`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/AddLayerObject/) |
| **`AddProtocolAction`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/AddProtocolAction/) |
| **`Alignment`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/Alignment/) |
| **`AnimationOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/AnimationOptions/) |
| **`AroundCenterOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/AroundCenterOptions/) |
| **`AttributionControlOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/AttributionControlOptions/) |
| **`BoxZoomEndHandler`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/BoxZoomEndHandler/) |
| **`BoxZoomHandlerOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/BoxZoomHandlerOptions/) |
| **`CalculateTileZoomFunction`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/CalculateTileZoomFunction/) |
| **`CameraForBoundsOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/CameraForBoundsOptions/) |
| **`CameraOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/CameraOptions/) |
| **`CameraUpdateTransformFunction`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/CameraUpdateTransformFunction/) |
| **`CanvasSourceSpecification`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/CanvasSourceSpecification/) |
| **`CenterZoomBearing`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/CenterZoomBearing/) |
| **`Complete<T>`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/Complete/) |
| **`ControlPosition`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/ControlPosition/) |
| **`Coordinates`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/Coordinates/) |
| **`CreateTileMeshOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/CreateTileMeshOptions/) |
| **`CustomLayerProjectionData`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/CustomLayerProjectionData/) |
| **`CustomLayerProjectionDataParams`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/CustomLayerProjectionDataParams/) |
| **`CustomRenderMethod`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/CustomRenderMethod/) |
| **`CustomRenderMethodInput`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/CustomRenderMethodInput/) |
| **`DashEntry`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/DashEntry/) |
| **`DistributiveKeys<T>`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/DistributiveKeys/) |
| **`DistributiveOmit<T, K extends DistributiveKeys<T>>`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/DistributiveOmit/) |
| **`DragPanOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/DragPanOptions/) |
| **`EaseToOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/EaseToOptions/) |
| **`ErrorEventType`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/ErrorEventType/) |
| **`EventTypeMap`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/EventTypeMap/) |
| **`EventedParentData`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/EventedParentData/) |
| **`ExpiryData`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/ExpiryData/) |
| **`FeatureIdentifier`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/FeatureIdentifier/) |
| **`FitBoundsOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/FitBoundsOptions/) |
| **`FlyToOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/FlyToOptions/) |
| **`FullscreenControlEventType`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/FullscreenControlEventType/) |
| **`FullscreenControlOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/FullscreenControlOptions/) |
| **`GeoJSONFeatureDiff`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/GeoJSONFeatureDiff/) |
| **`GeoJSONFeatureId`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/GeoJSONFeatureId/) |
| **`GeoJSONSourceDiff`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/GeoJSONSourceDiff/) |
| **`GeolocateControlEventType`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/GeolocateControlEventType/) |
| **`GeolocateControlOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/GeolocateControlOptions/) |
| **`GestureOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/GestureOptions/) |
| **`GetClusterOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/GetClusterOptions/) |
| **`GetResourceResponse<T>`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/GetResourceResponse/) |
| **`GlyphPosition`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/GlyphPosition/) |
| **`GlyphPositions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/GlyphPositions/) |
| **`HandlerResult`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/HandlerResult/) |
| **`ImageSourceImage`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/ImageSourceImage/) |
| **`ImageSourceWarp`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/ImageSourceWarp/) |
| **`IndicesType`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/IndicesType/) |
| **`JumpToOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/JumpToOptions/) |
| **`Listener<E extends Event = Event>`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/Listener/) |
| **`LngLatBoundsLike`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/LngLatBoundsLike/) |
| **`LngLatLike`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/LngLatLike/) |
| **`LogoControlOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/LogoControlOptions/) |
| **`MapEventType`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/MapEventType/) |
| **`MapGeoJSONFeature`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/MapGeoJSONFeature/) |
| **`MapLayerEventType`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/MapLayerEventType/) |
| **`MapLayerMouseEvent`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/MapLayerMouseEvent/) |
| **`MapLayerTouchEvent`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/MapLayerTouchEvent/) |
| **`MapOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/MapOptions/) |
| **`MapSourceDataType`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/MapSourceDataType/) |
| **`MarkerEventType`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/MarkerEventType/) |
| **`MarkerOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/MarkerOptions/) |
| **`Mat4f32`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/Mat4f32/) |
| **`Mat4f64`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/Mat4f64/) |
| **`MissingStyleImageResolver`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/MissingStyleImageResolver/) |
| **`NavigationControlOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/NavigationControlOptions/) |
| **`Offset`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/Offset/) |
| **`PaddingOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/PaddingOptions/) |
| **`PointLike`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/PointLike/) |
| **`PopupEventType`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/PopupEventType/) |
| **`PopupOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/PopupOptions/) |
| **`PositionAnchor`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/PositionAnchor/) |
| **`ProjectionData<MainMatrix extends mat4 = mat4, FallbackMatrix extends mat4 = MainMatrix>`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/ProjectionData/) |
| **`ProjectionDataParams`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/ProjectionDataParams/) |
| **`QueryRenderedFeaturesOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/QueryRenderedFeaturesOptions/) |
| **`QuerySourceFeatureOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/QuerySourceFeatureOptions/) |
| **`RendererProjectionData`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/RendererProjectionData/) |
| **`RequestParameters`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/RequestParameters/) |
| **`RequestResponseMessageMap`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/RequestResponseMessageMap/) |
| **`RequestTransformFunction`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/RequestTransformFunction/) |
| **`RequireAtLeastOne<T>`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/RequireAtLeastOne/) |
| **`ScaleControlOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/ScaleControlOptions/) |
| **`SetClusterOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/SetClusterOptions/) |
| **`SourceClass`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/SourceClass/) |
| **`SourceEventType`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/SourceEventType/) |
| **`StyleGlyph`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/StyleGlyph/) |
| **`StyleImage`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/StyleImage/) |
| **`StyleImageData`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/StyleImageData/) |
| **`StyleImageMetadata`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/StyleImageMetadata/) |
| **`StyleImageSource`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/StyleImageSource/) |
| **`StyleImageWebGLData`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/StyleImageWebGLData/) |
| **`StyleImageWebGLTarget`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/StyleImageWebGLTarget/) |
| **`StyleOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/StyleOptions/) |
| **`StyleSetterOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/StyleSetterOptions/) |
| **`StyleSwapOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/StyleSwapOptions/) |
| **`TileMesh`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/TileMesh/) |
| **`TransformConstrainFunction`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/TransformConstrainFunction/) |
| **`TransformStyleFunction`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/TransformStyleFunction/) |
| **`Unit`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/Unit/) |
| **`UnwrappedTileIDLiteral`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/UnwrappedTileIDLiteral/) |
| **`UpdateImageOptions`** | [Docs &rarr;](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/UpdateImageOptions/) |

---
