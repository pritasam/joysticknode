##A Simple Virtual Joystick made for the Apple Sprite Kit Framework

Creates a virtual onscreen joystick composed of 2 circles which you can configure with any sprite name you'd like.  Options include ability to dynamically show the control when the user taps and center itself on the tap or can be used as a fixed controller

The sample application shows how to use them in a simple move/shoot scenario similar to a lot of games of this style.

## Basic Usage

```objc
KAZ_JoystickNode *moveJoystick = [[KAZ_JoystickNode alloc] init];
[moveJoystick setOuterControl:@"outer" withAlpha:0.25];
[moveJoystick setInnerControl:@"inner" withAlpha:0.5];
moveJoystick.speed = 8;
[self addChild:moveJoystick];
```

This will create the joystick with using an image named 'outer' for the outer circle and a 0.25 alpha, and an image named 'inner' for the inner circle at 0.5 alpha.  The speed will be scaled from 0-8.

## Basic Flow

On your scene, you will start control on touchesBegan

```objc
-(void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event {
    for (UITouch *touch in touches) {
        CGPoint location = [touch locationInNode:self];
        // Add some conditional to determine if the touch is one you want to use to control the joystick
        if ( location.x < 200 && location.y < 200 ){
          [moveJoystick startControlFromTouch:touch andLocation:location];
        }
    }
}
```

This will tell the control to show and start controlling using the specified point and location if they tap in the lower area of the screen

Then during touch moves, you'll update the control with:

```objc
-(void)touchesMoved:(NSSet *)touches withEvent:(UIEvent *)event{
    for (UITouch *touch in touches) {
        if ( touch == moveJoystick.startTouch){
            [moveJoystick moveControlToLocation:touch andLocation:[touch locationInNode:self]];
        }
    }
}
```

This will tell the joystick of the new movements

Then to end control, you'll do:

```objc
-(void)touchesEnded:(NSSet *)touches withEvent:(UIEvent *)event{
    for (UITouch *touch in touches) {
        if ( touch == moveJoystick.startTouch){
            [moveJoystick endControl];
        }
    }
}
```

## Properties

### isMoving

Flag to know when the control is registering movements

## autoShowHide

Defaults to YES.  Will show the control when the start touch gets registered

## moveSize

The size of the movement the control calculates.  Based on the speed and angle, it'll generate a movement in the x/y space for you to apply to any node.

## startTouch

The touch to register movement with the control

## speed

The amount you want to adjust the move size by.  Will be scaled based on the distance from the center point to the outer ring

## angle

The angle being reported by the movment of the inner circle.  0 is directly to the right, 180 is is directly to left, 90 is at the top, and -90 is at the bottom

## defaultAngle

The angle to report if there is no movement, the control will reset to this
