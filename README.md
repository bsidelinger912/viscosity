#Viscosity

A lightweight sass library for creating fluid layouts driven by aspect ratios that evolve as the window widens. In a situation like a hero image at the top of a page, often you want something like a 1x1 aspect ratio on phones, but want something like a 16x9 ratio on desktop.  Transitioning fluidly between these points can create nasty height changes that push content lower in the page around. Viscosity generates smooth transitions between aspect ratios by calculating the correct break points to make the transitions without jumping the height around.

Here's an example of what a transition between 1x1, 3x2, and 16x9 aspect ratios looks like (the animation plays five times):

![viscosity transition example](examples/image_demo/rectangle.gif)

##Getting Started
These instructions assume you already have node and npm installed, and a sass build set up.

1. Install with npm `npm install --save viscosity`
2. Import into your sass files `@import "~viscosity/src/viscosity";`

##Usage

Viscosity is used with a list of maps that define the ratios you want to hit.

```
@import "~viscosity/src/viscosity";

$breakList1: (
  (aspectWidth: 1, aspectHeight: 1, maxHeight: 400, leftRightPadding: 16),
  (aspectWidth: 3, aspectHeight: 2, maxHeight: 600, leftRightPadding: 16),
  (aspectWidth: 16, aspectHeight: 9, maxHeight: 0, leftRightPadding: 16),
);

.viscous1 {
  @include viscosity($breakList1);
}
```

A couple other mixins are available too
```
// media queries as a mixin with the media() mixin.  This keeps your styling for an element in one place
.myElem {
  @include media(0, 600px) {
    width: 100%;
  }
  @include media(600px) {
    width: 50%;
  }
}

// conform to aspect ratio with aspectRatio() mixin
.myElem {
  @include aspectRatio(16,9);
}
```

Viscosity also comes with a handy function that gives you access to the raw break point data.  This allows you to use the breakpoints to add custom style rules as your element transitions.

```
@import "~viscosity/src/viscosity";

$breakList1: (
  (aspectWidth: 1, aspectHeight: 1, maxHeight: 400, leftRightPadding: 16),
  (aspectWidth: 3, aspectHeight: 2, maxHeight: 600, leftRightPadding: 16),
  (aspectWidth: 16, aspectHeight: 9, maxHeight: 0, leftRightPadding: 16),
);

$allBreakPoints: addBreakWidths($breakList1);

.viscous1 {
  @include viscosity($breakList1);

  @for $i from 1 through length($allBreakPoints) {
    $break: nth($allBreakPoints, $i);

    @include media(map-get($break, minWidth), map-get($break, maxWidth)) {
      @if(i == 0) {
        .child {
          position: absolute;
          left: 0;
          bottom: 0;
          width: 100%;
          text-align: center;
        }
      } else {
        .child {
          float: left;
        }
      }
    }
  }
}
```

See the examples folder for more usage demonstration.
