
# react-native-responsive-layout

Set of components and utilities that make building responsive RN user interfaces easy by bringing concepts used on the web.

## Installation

This package is only **compatible with RN>=42**, as older versions do not support percentage-based widths.  
To install the latest version simply run:

```bash
yarn add react-native-responsive-layout
```
or if you prefer using `npm`:
```bash
npm install --save react-native-responsive-layout
```

## Responsive layout

Even though React-Native offers a great way to build complex native applications fast, building responsive UI still isn't as easy as on the web. With RN42, things got drastically better when percentage based widths landed. Still building responsive applications is quite hard without introducing many conditional renderings, this framework aims to bring many concepts we are used to on web to simplify native development.

The most important concept these components bring is **ability to specify different component sizes and to select styles depending on viewport size** to enable responsive making building responsive elements as easy as using Bootstrap grid.

It is built to be **mobile first**, so grid collapses to largest size class that it fits. This way you don't need to define sizes/styles for all breakpoint sizes, rather you can implement only those that matter and it will fallback to the latest one that it fits. For example, you can define only `xsSize` and `lgSize`, on all sizes lower than large it will fallback to extra small, but on all larger or equal to large, it will pick it. This gives you the flexibility to target most device sizes quite precisely.

## Usage Example



## Size Classes

Sizing is mobile first so it renders depending on current size and fallbacks to lower sizes for missing breakpoint values. Therefore there is no need to explicitly define all sizes, it is possible only to target breakpoints you care about.

Based on currently popular device point sizes, grid breakpoints are chosen so it would be possible to precisely target devices of all sizes. 

Most notable **differences compared to CSS frameworks** are that we differentiate two portrait sizes for mobile devices since in many cases 100 points difference which covers almost 1/4th of the screen could be used to render things differently.

The second difference is that we are not interested in desktop sizes so we can also have more break points on large devices where there could also be a significant difference in sizes.

Based on popular device sizes grid breakpoints are divided as following:

 **Mobile** - iPhone 5/SE ( `320x568` ), iPhone 7/8 ( `375x667` ), Galaxy S6/S7 ( `360x640` )  
 **Mobile Large** - iPhone 7+/8+ ( `414x736` ) , Nexus 5X/Pixel ( `411x731` )  
 **Tablet** - iPad Mini/Air ( `768x1024` ), Nexus 9 ( `1024x768` )  
 **Tablet Large** - iPad Pro 12,9 ( `1024x1366` )


| xs     | sm           | md               | lg     | xl               | xxl                    |
|--------|--------------|------------------|--------|------------------|------------------------|
| 320    | >= 411       | >= 568           | >= 768 | >= 1024          | >= 1280                |
| Mobile | Mobile Large | Mobile Landscape | Tablet | Tablet Landscape | Tablet Large Landscape |
|        |              |                  |        | Tablet Large     |                        |


# API Reference
## Components

### Grid
A component which contains sections and boxes. It is the source for of breaking points and size that is used as a reference for breakpoints.

- **breakpoints** - object containing sizes upon child's boxes will collapse, if you don't provide some of them it will automatically fallback to the first previous smaller, by default values are from table above:
  - **xs**: 320
  - **sm**: 411
  - **md**: 568
  - **lg**: 768
  - **xl**: 1024
  - **xxl**: 1280
- **relativeTo** (_default = window_) - whether to use breakpoints based on container size or viewport size
  - **window** - any changes to window size will trigger sizing recalculation and re-render, this will also handle when screen rotates
  - **self** - size classes will be calculated based on the grid container size, this can be useful in cases when you have only part of screen that you want to be responsive
- **direction** (_default = vertical_) - direction in which sections will be laid out, it goes hand in hand with `horizontal` property of `ScrollView`
  - **horizontal**
  - **vertical**


### Section
Section of grid elements, in default grid direction (vertical) it is same as a row in web-based grid systems. Its purpose is to group elements (boxes) together, enable breaking into new row and expansion of auto-expanding elements into remaining space.

### Box
The smallest building block of grid elements. It renders itself depending on grid size.

- **size** (_default="1/1"_) - used when there are no explicit sizes provided making box fixed width percentage across all the resolutions
- **xsSize, smSize, mdSize, lgSize, xlSize, xxlSize** - size used when specific size class is active since this grid is mobile first and cascades from smaller to larger sizes, it will choose class that is lowest resolution larger than component/window size
  - `"auto"` - when used it will stretch to be maximum width to fill remaining space surrounding other boxes, can be useful to fill spaces and to align components right
  - **numeric percentage** from 0 to 100% (eg. `30`)
  - **string fraction**, grid is based on 12's (eg. `'1/2'`, `'3/4'`... `'1/12'`)
- **xsHidden, smHidden, mdHidden, lgHidden, xlHidden, xslHidden** - just like sizes, it will hide element attribute depending on current size

## Wrappers
### withSize(Component) -> Component

Provides component with size attributes of outer grid.
- **size** - provides first outer grid size class (eg. `sm`, `lg`...)
- **sizeSelector** - depending on current grid size, selects relevant value from object, that contains sizes as keys, it is possible to provide only some of them, just like rest of grid, it will fallback to first smaller that satisfies criteria, especially useful when using with styles since it enables selection of appropriate style to match the box size (eg. you can create matching `lgSize` and `lg` style)
- **orientation** - reference object orientation, either window or the grid itself (based on `relativeTo` configuration of Grid), enables targeting specific device orientation when used relative to viewport

### withContainerDimensions(Component) -> Component

Provides component with current `width` and `height` of object grid sizes are relative to (either viewport or grid itself).

- **width** - reference component width
- **height** - reference component height

## Utility Functions

### calculateStretchLength(totalLength, minimalElementLength) -> length

Calculates element size to ensure that elements are proportionally stretched so the maximum amount of elements fits total size and size never goes below minimal element size. This is useful for building grids that have objects of equal width/height and have the specific size. The best example is tiled image galery.