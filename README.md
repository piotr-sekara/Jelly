
# Jelly
#### Create rich viewcontroller transition animations with just a few lines of code

![Jelly-Animators: Elegant Viewcontroller Animations in Swift](https://github.com/SebastianBoldt/Jelly/blob/master/Github/Jellyfish.png)

<a href="https://cocoapods.org/pods/Jelly"><img src="https://img.shields.io/badge/version-2.0.0-green.svg?longCache=true&style=flat-square" alt="current version" /></a>
<a href="http://twitter.com/sebastianboldt"><img src="https://img.shields.io/badge/twitter-@sebastianboldt-blue.svg?longCache=true&style=flat-square" alt="twitter handle" /></a>
<a href="https://developer.apple.com/swift"><img src="https://img.shields.io/badge/swift4.2-compatible-orange.svg?longCache=true&style=flat-square" alt="Swift 4.2 compatible" /></a>
<a href="https://www.apple.com/de/ios/ios-11/"><img src="https://img.shields.io/badge/platform-iOS-lightgray.svg?longCache=true&style=flat-square" alt="platform" /></a>
<a href="https://github.com/Carthage/Carthage"><img src="https://img.shields.io/badge/carthage-compatible-green.svg?longCache=true&style=flat-square" alt="carthage support" /></a>
<a href="https://en.wikipedia.org/wiki/MIT_License"><img src="https://img.shields.io/badge/license-MIT-lightgray.svg?longCache=true&style=flat-square" alt="license" /></a>

Jelly provides custom view controller transitions with just a few lines of code.
No need to create your own Presentation-Controller or Animator objects.
An Animator will do the heavy lifting for you.

```swift

var shiftInPresentation = ShiftInPresentation(directionShow: .left)
let animator = Animator(presentation:presentation)
animator.prepare(viewController: viewController)
present(viewController, animated: true, completion: nil)
```
## 📱 Example

You can use Jelly to build your own Alertviews or Slidein-Menus using ViewControllers designed by yourself.

![Jelly-Animators: Elegant Viewcontroller Animations in Swift](https://github.com/SebastianBoldt/Jelly/blob/master/Github/notification.gif?raw=true)   ![Jelly-Animators: Elegant Viewcontroller Animations in Swift](https://github.com/SebastianBoldt/Jelly/blob/master/Github/slideover.gif?raw=true)

![Jelly-Animators: Elegant Viewcontroller Animations in Swift](https://github.com/SebastianBoldt/Jelly/blob/master/Github/shiftindimmed.gif?raw=true)  ![Jelly-Animators: Elegant Viewcontroller Animations in Swift](https://github.com/SebastianBoldt/Jelly/blob/master/Github/shiftinblurred.gif?raw=true)

![Jelly-Animators: Elegant Viewcontroller Animations in Swift](https://github.com/SebastianBoldt/Jelly/blob/master/Github/fadin.gif?raw=true)  ![Jelly-Animators: Elegant Viewcontroller Animations in Swift](https://github.com/SebastianBoldt/Jelly/blob/master/Github/blurredslidein.gif?raw=true)


To run the example project, clone  the repo, and run `pod install` from the Example directory first.

## 🔧How to use

Jelly is super easy to use.

1. Create a *Presentation* Object (SlideIn, FadeIn or ShiftIn)
2. Initialize a *Animator* using the *Presentation* Object created in Step 1.
3. Call the *prepare(viewController:UIViewController)* Function
4. Finally call the native *UIViewController* presentation function.

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    let viewController = self.storyboard.instantiateViewController(withIdentifier: "someViewController")
    //1.
    let presentation = Jelly.SlideInPresentation()
    //2.
    self.animator = Jelly.Animator(presentation:presentation)
    //3.
    self.animator?.prepare(viewController: viewController)
    //4.
    self.present(viewController, animated: true, completion: nil)
}

```

***DO NOT FORGET TO KEEP A STRONG 💪 REFERENCE***

Because the *transitioningDelegate* of a *UIViewController* is weak, you need to
hold a strong reference to the *Animator* inside the *UIViewController* you are presenting from or the central object that maintains your presentations.

```swift
class CustomVC : UIViewController {
    var animator: Jelly.Animator?
    override func viewDidLoad() {
        super.viewDidLoad()
        var shiftInPresentation = Jelly.ShiftInPresentation()
        shiftInPresentation.direction = .left
        let animator = Jelly.Animator(presentation:presentation)
        self.animator = animator
    }
}
```

That's it. That's lit.

## 🖌 Customize
Jelly offers 3 types of Presentations for you:
* **SlideInPresentation**
* **ShiftInPresentation**
* **FadeInPresentation**

Not every property is available for each animation.
Check out the interfaces of each class to learn more about them.

* **duration:** Constants.Duration (default: normal)
    * ultraSlow = 2.0
    * slow = 1.0
    * medium = 0.5
    * normal = 0.35
    * fast = 0.2
    * reallyFast = 0.1
* **backgroundStyle:** Constants.BackgroundStyle (default: .dimmed(0.5))
    * dimmed(alpha: CGFloat)
    * blur(effectStyle: UIBlurEffectStyle)
    * if you want a transparent background use .dimmed(alpha:0.0)
* **cornerRadius:** Double (default: 0)
* **corners:** UIRectCorner (default: .allCorners)
    * define which corners the radius should be applied to
* **presentationCurve:** Constants.Curve (default: linear)
    * easeIn
    * easeOut
    * easeInEaseOut
    * linear
* **dismissCurve:** Constants.Curve (default: linear)
    * easeIn
    * easeOut
    * easeInEaseOut
    * linear
* **isTapBackgroundToDismissEnabled** (default: true)
    * tapping the background dismisses the ViewController by default
    * set it to false to prevent this behavior
* **widthForViewController:** Constants.Size (default: fullscreen)
    * If the container is smaller than the provided width, Jelly will automatically resize to the containers width
    * if Margin Guards are specified they also will be applied if width is to wide for the container
* **heightForViewController:** Constants.Size (default: fullscreen)
    * If the container is smaller than the provided height, Jelly will automatically resize to the containers width
    * if Margin Guards are specified they also will be applied when height is to high for the container
* **horizontalAlignment:** Constants.HorizontalAlignment (default: .center)
    * center, left or right
* **verticalAlignemt:** Constants.VerticalAlignment (default:center)
    * top, bottom, center
* **marginGuards:** default(UIEdgeInsets.zero)
    * If the width or height is bigger than the container we are working with, marginGuards will kick in and limit the size using the specified margins
* **directionShow:** Constants.Direction (default: top)
    * left, top, bottom, right
* **directionDismiss:** Constants.Direction (default: top)
    * left, top, bottom, right
* **spring:** (default: none)
    * none (damping = 1.0, velocity = 0.0)
    * tight (damping = 0.7, velocity = 2)
    * medium (damping = 0.5 , velocity = 3)
    * loose (damping = 0.2, velocity = 4)

```swift

let customPresentation = Jelly.SlideInPresentation(dismissCurve: .linear,
                                                    presentationCurve: .linear,
                                                         cornerRadius: 15,
                                                      backgroundStyle: .blur(effectStyle: .light),
                                                               spring: .medium,
                                                             duration: .normal,
                                                        directionShow: .top,
                                                     directionDismiss: .top,
                                               widthForViewController: .fullscreen,
                                              heightForViewController: .custom(value:200) ,
                                                  horizontalAlignment: .center,
                                                    verticalAlignment: .top,
                                                         marginGuards: UIEdgeInsets(top: 0, left: 10, bottom: 0, right: 10),
                                                              corners: [.topLeft,.bottomRight])


self.animator = Jelly.Animator(presentation:customPresentation)
self.animator?.prepare(viewController: viewController)
self.present(viewController, animated: true, completion: nil)
```

## Jelly and Hero 😍

Coming soon ...

## ✅ Requirements

Deployment target of your App is >= iOS 8.0

## 📲 Installation

Jelly is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod "Jelly"
```
## 🗣 Mentions

* Mentioned in <i>iOS Dev Weekly</i> by <a href="https://twitter.com/daveverwer">@Dave Verwer</a> - <a href="http://iosdevweekly.com/issues/279"> Issue NO. 112 </a>
* Mentioned in <i>This Week in Swift</i> by <a href="https://twitter.com/NatashaTheRobot">@Natasha the Robot</a> - <a href="https://swiftnews.curated.co/issues/112#start"> Issue No. 279 </a>

## 🤖 Author

Sebastian Boldt, www.sebastianboldt.com

## 📄 License

Jelly is available under the MIT license. See the LICENSE file for more info.
