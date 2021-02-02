## Recreating the iconic Star Wars Intro in Flutter

### "I am your Flutter" — Dart Vader
I love Star Wars and I love Flutter. So after a brief hiatus from trying fun stuff with Flutter, and looking for good motivation to start writing code again, *as well as some other personal reason* 😉, I came up with this brilliant idea, *as many before me have, apparently*, to recreate the intro to the Star Wars movies in code. I did it with Flutter! 

If you're still here then my bad jokes didn't bore you away and you're obviously interested, so let's dive in 🤿. 

### Breakdown of components
There are five main elements in the intro
- The intro text
- The starry background
- The Star Wars logo
- The crawling text
- The music

We'll create widgets for all these components and animate them all together with Flutter's helpful Tween and Staggered animations.

*Let's get started!*

### The intro text
A long time ago, but somehow in the future? The Star Wars intro begins with this famous phrase. Let's build a widget for it.

```
class IntroText extends StatelessWidget {
  IntroText({this.opacity});

  // This variable will be responsible for animating the opacity of this widget
  final Animation<double> opacity;

  @override
  Widget build(BuildContext context) {
    return Opacity(
      // We adjust the opacity based on the value of the animation variable
      opacity: opacity.value,
      child: Center(
        child: FittedBox(
          child: Padding(
            padding: const EdgeInsets.all(40.0),
            child: Text(
              'A long time ago in a galaxy far,\nfar away....',
              style: TextStyle(
                color: Color(0xFF11B2A3),
                fontSize: 72.0,
              ),
            ),
          ),
        ),
      ),
    );
  }
}
```

Here we go...

![The intro text showing in a browser window](https://cdn.hashnode.com/res/hashnode/image/upload/v1612058949414/6AhG9SFgW.jpeg)

**Note - ** In order to display these widgets before the animation logic has been written you'd have to pass a temporary `AlwaysStoppedAnimation<double>()` value to the constructor of the widget. 

For example:

```
class TempView extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Material(
      type: MaterialType.transparency,
      child: Container(
        color: Colors.black,
        child: Stack(
          children: [
            IntroText(
              opacity: AlwaysStoppedAnimation<double>(1),
            ),
          ],
        ),
      ),
    );
  }
}
```

### The starry background
The background of the Star Wars intro is a simple black void of space dotted with stars. Instead of using a boring static image, let's create it ourselves.

#### Twinkle twinkle little star 🌟
First, we'll create a star widget. This will be a simple, tiny Container widget.

```
class TwinkleLittleStar extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      width: 1.0,
      height: 1.0,
      decoration: BoxDecoration(
        color: Colors.white,
        shape: BoxShape.circle,
      ),
    );
  }
}
```
#### "Space: the final frontier."
*oops, wrong franchise* 😅  
Anyway, let's go ahead and generate a bunch of these stars in our `Space` widget.

```
import 'dart:math' as math;

...

// Our Space widget is stateful so that we can generate a bunch of random stars
// only once when the widget loads by overriding the didChangeDependencies method
class Space extends StatefulWidget {
  Space({this.opacity});

  final Animation<double> opacity;

  @override
  _SpaceState createState() => _SpaceState();
}

class _SpaceState extends State<Space> {
  final int _starCount = 300;

  List<Widget> _stars;

  @override
  void didChangeDependencies() {
    super.didChangeDependencies();

    // We generate our stars in this method so that it builds only once
    // but after the initState method has finished running.
    _stars = _generateStars();
  }

  List<Widget> _generateStars() {
    return List.generate(_starCount, (index) {
      List<double> xy = _getRandomPosition(context);
      return Positioned(
        top: xy[0],
        left: xy[1],
        child: TwinkleLittleStar(),
      );
    });
  }

  List<double> _getRandomPosition(BuildContext context) {
    // We get the dimensions of the screen and use them to generate random coordinates
    double x = MediaQuery.of(context).size.height;
    double y = MediaQuery.of(context).size.width;

    double randomX =
        double.parse((math.Random().nextDouble() * x).toStringAsFixed(3));
    double randomY =
        double.parse((math.Random().nextDouble() * y).toStringAsFixed(3));

    return [randomX, randomY];
  }

  @override
  Widget build(BuildContext context) {
    return Opacity(
      opacity: widget.opacity.value,
      child: Material(
        type: MaterialType.transparency,
        child: Container(
          color: Colors.black,
          child: Stack(
            children: _stars,
          ),
        ),
      ),
    );
  }
}
```

That's it! We have our starry background 🌌. You can see our stars twinkling below, *if you squint hard enough.*

![The starry background showing in a browser window](https://cdn.hashnode.com/res/hashnode/image/upload/v1612058975506/cUSeYilsc.jpeg)

### The Star Wars logo
Don't we all love it when the Star Wars logo jumps out of nowhere right on cue with the soundtrack? We can make that happen too but first we need the logo.

The logo we're using can be found at this [link](https://logos-download.com/wp-content/uploads/2016/09/Star_Wars_logo-1.png).

```
class StarWarsLogo extends StatelessWidget {
  StarWarsLogo({this.size, this.opacity});

  // This time there is an extra animation variable which will animate the size of the logo,
  // progressively making it smaller and smaller to simulate it moving away.
  final Animation<double> size;
  final Animation<double> opacity;

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Opacity(
        opacity: opacity.value,
        // We get the image straight from the net. This is a sci-fi after all...
        child: Image.network(
          'https://logos-download.com/wp-content/uploads/2016/09/Star_Wars_logo-1.png',
          width: size.value,
          fit: BoxFit.fitHeight,
        ),
      ),
    );
  }
}
```
Here's what we get:

![The Star Wars logo showing in a browser window](https://cdn.hashnode.com/res/hashnode/image/upload/v1612058998714/EGQmYPBS5.jpeg)

### The crawling text
What better way to deliver exposition than in bright yellow perspective text that slowly crawls up from the bottom of your screen! Let's see how we can create this widget.

```
class CrawlingText extends StatelessWidget {
  CrawlingText({this.topMargin, this.bottomMargin, this.opacity});

  // The `topMargin` and `bottomMargin` values are what are used to acheive the crawling effect
  final Animation<double> topMargin;
  final Animation<double> bottomMargin;
  final Animation<double> opacity;

  final TextStyle _crawlingTextStyle = TextStyle(
    color: Color(0xFFFFC500),
    fontSize: 40.0,
    fontWeight: FontWeight.bold,
  );

  @override
  Widget build(BuildContext context) {
    final double maxWidthConstraint = MediaQuery.of(context).size.width * 0.6;

    return Opacity(
      opacity: opacity.value,
      child: Center(
        child: Transform(
          // This transformation adjusts it's child to achieve the perspective angle we want
          transform: Matrix4.identity()
            ..setEntry(3, 2, 0.007)
            ..rotateX(5.6),
          alignment: FractionalOffset.center,
          child: Container(
            // Adjust the margins above and below the text to make it simulate upward movement
            margin: EdgeInsets.only(
                top: topMargin.value, bottom: bottomMargin.value),
            child: FittedBox(
              child: Container(
                constraints: BoxConstraints(
                  maxWidth: maxWidthConstraint < 500
                      ? MediaQuery.of(context).size.width
                      : maxWidthConstraint,
                ),
                padding: EdgeInsets.symmetric(horizontal: 64.0),
                child: Column(
                  children: [
                    Text(
                      'Episode VII\nTHE FORCE AWAKENS',
                      textAlign: TextAlign.center,
                      style: _crawlingTextStyle,
                    ),
                    Text(
                      "\nLuke Skywalker has vanished. In his absence, the sinister FIRST ORDER has risen from the ashes of the Empire and will not rest until Skywalker, the last Jedi, has been destroyed."
                      "\n\nWith the support of the REPUBLIC, General Leia Organa leads a brave RESISTANCE. She is desperate to find her brother Luke and gain his help in restoring peace and justice to the galaxy."
                      "\n\nLeia has sent her most daring pilot on a secret mission to Jakku, where an old ally has discovered a clue to Luke's whereabouts.…",
                      textAlign: TextAlign.justify,
                      style: _crawlingTextStyle,
                    ),
                  ],
                ),
              ),
            ),
          ),
        ),
      ),
    );
  }
}
```

The result will look something like this depending on the values you provide for the margins

![The crawling text showing in a browser window](https://cdn.hashnode.com/res/hashnode/image/upload/v1612060928122/Q2exKjfE6.jpeg)

And finally,

### The music
There's no great movie without a great soundtrack and that of Star Wars is no exception. In order to add audio to our little project we're going to need the [audioplayers](https://pub.dev/packages/audioplayers) package.

This package allows us to stream audio directly from the net and that's exactly what we're going to do by creating our own `MyAudioPlayer` class.

The music we're using can be found at this [link](https://s.cdpn.io/1202/Star_Wars_original_opening_crawl_1977.mp3).

```
import 'package:audioplayers/audioplayers.dart';

...

class MyAudioPlayer {
  static AudioPlayer audioPlayer = AudioPlayer();

  static Future<void> playMusic() async {
    int result = await audioPlayer.play(
        "https://s.cdpn.io/1202/Star_Wars_original_opening_crawl_1977.mp3");
    if (result == 1) {
      // success
    }
  }

  static Future<void> stopMusic() async {
    int result = await audioPlayer.stop();
    if (result == 1) {
      // success
    }
  }
}
```
We can now use the ▶ `playMusic()` and ⏹ `stopMusic()` methods to control the music.

### Animating the widgets
Now that we have all the components, we can begin animating them.

In order to animate these widgets one after the other or with overlapping animations we need a form of key-framing logic. Thankfully, Flutter has its own version of this called Staggered animations and it uses a bunch of Tweens all controlled by a single animation controller to animate the widgets in the order you specify.

To achieve our animation we will need to 
- Create an `AnimationController` that manages all of the Animations.
- Create a `Tween` for each property being animated. The `Tween` defines a range of values. Also, the Tween’s animate method requires the parent controller, and produces an Animation for that property.
- Specify the interval on the Animation’s curve property.

#### Creating the AnimationController
We'll create the `AnimationController` in our `home` widget. It'll be a `StatefulWidget` that extends `TickerProviderStateMixin` as required by the `AnimationController`.

```
import 'package:flutter/scheduler.dart' show timeDilation;

...

class StarWarsIntro extends StatefulWidget {
  @override
  _StarWarsIntroState createState() => _StarWarsIntroState();
}

class _StarWarsIntroState extends State<StarWarsIntro>
    with TickerProviderStateMixin {
  AnimationController _controller;

  @override
  void initState() {
    super.initState();

    _controller = AnimationController(
      duration: const Duration(milliseconds: 1000),
      vsync: this,
    )..addListener(_controllerListener);
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  // We listen to the controller and stop the music when the animation ends
  void _controllerListener() {
    if (_controller.value == 1) MyAudioPlayer.stopMusic();
  }

  // The animation will play to the end and then reset
  Future<void> _playAnimation() async {
    try {
      await _controller.forward().orCancel;
      _controller.reset();
    } on TickerCanceled {
      // the animation got canceled, probably because it was disposed of
    }
  }

  @override
  Widget build(BuildContext context) {
    timeDilation = 80.0; // Time Dilation reduces the speed of the animation
    return Material(
      type: MaterialType.transparency,
      child: Stack(
        children: [
          GestureDetector(
            behavior: HitTestBehavior.opaque,
            onTap: () {
              // We simultaneously play the animation and the music when a tap is recognized
              _playAnimation();
              MyAudioPlayer.playMusic();
            },
            child: StarWarsIntroAnimation(
              controller: _controller,
              size: MediaQuery.of(context).size,
            ),
          ),
        ],
      ),
    );
  }
}
```

#### Creating Tweens and specifying the interval of animation

Next, we'll create a `StatelessWidget` in which we'll create the Tweens of all the properties being animated. The `build()` function of this widget instantiates an AnimatedBuilder.

AnimatedBuilder listens to notifications from the animation controller, marking the widget tree dirty as values change and calls our function `_buildAnimation()` which rebuilds the UI for each tick of the animation.

```
class StarWarsIntroAnimation extends StatelessWidget {
  StarWarsIntroAnimation({Key key, this.controller, this.size})
      // Each property that is going to be animated has a `Tween` created for it
      // The main AnimationController is passed to each of them and their
      // interval of animation is specified
      : introTextOpacityShow = Tween<double>(
          begin: 0.0,
          end: 1.0,
        ).animate(
          CurvedAnimation(
            parent: controller,
            // The interval are two values from 0.0 to 1.0 which will take up a
            // fraction of the AnimationCOntroller's total interval and
            // specify at which values of the AnimationController this `Tween` will animate
            curve: Interval(
              0.0,
              0.01,
              curve: Curves.ease,
            ),
          ),
        ),
        introTextOpacityHide = Tween<double>(
          begin: 1.0,
          end: 0.0,
        ).animate(
          CurvedAnimation(
            parent: controller,
            curve: Interval(
              0.08,
              0.1,
              curve: Curves.ease,
            ),
          ),
        ),
        spaceOpacity = Tween<double>(
          begin: 0.0,
          end: 1.0,
        ).animate(
          CurvedAnimation(
            parent: controller,
            curve: Interval(
              0.12,
              0.12,
              curve: Curves.ease,
            ),
          ),
        ),
        starWarsLogoOpacityShow = Tween<double>(
          begin: 0.0,
          end: 1.0,
        ).animate(
          CurvedAnimation(
            parent: controller,
            curve: Interval(
              0.12,
              0.12,
              curve: Curves.ease,
            ),
          ),
        ),
        starWarsLogoOpacityHide = Tween<double>(
          begin: 1.0,
          end: 0.0,
        ).animate(
          CurvedAnimation(
            parent: controller,
            curve: Interval(
              0.25,
              0.3,
              curve: Curves.ease,
            ),
          ),
        ),
        starWarsLogoSize = Tween<double>(
          begin: size.width,
          end: 0.0,
        ).animate(
          CurvedAnimation(
            parent: controller,
            curve: Interval(
              0.09,
              0.35,
              curve: Curves.decelerate,
            ),
          ),
        ),
        crawlingTextPositionTop = Tween<double>(
          begin: size.height,
          end: 0.0,
        ).animate(
          CurvedAnimation(
            parent: controller,
            curve: Interval(
              0.0,
              0.8,
              curve: Curves.linear,
            ),
          ),
        ),
        crawlingTextPositionBottom = Tween<double>(
          begin: 0.0,
          end: size.height * 0.7,
        ).animate(
          CurvedAnimation(
            parent: controller,
            curve: Interval(
              0.4,
              1.0,
              curve: Curves.linear,
            ),
          ),
        ),
        crawlingTextOpacity = Tween<double>(
          begin: 1.0,
          end: 0.0,
        ).animate(
          CurvedAnimation(
            parent: controller,
            curve: Interval(
              0.9,
              1.0,
              curve: Curves.linear,
            ),
          ),
        ),
        super(key: key);

  final Size size;
  final AnimationController controller;
  final Animation<double> spaceOpacity;
  final Animation<double> introTextOpacityShow;
  final Animation<double> introTextOpacityHide;
  final Animation<double> starWarsLogoOpacityShow;
  final Animation<double> starWarsLogoOpacityHide;
  final Animation<double> starWarsLogoSize;
  final Animation<double> crawlingTextOpacity;
  final Animation<double> crawlingTextPositionTop;
  final Animation<double> crawlingTextPositionBottom;

  Widget _buildAnimation(BuildContext context, Widget child) {
    return Container(
      color: Colors.black,
      child: Stack(
        children: [
          Space(opacity: spaceOpacity),
          IntroText(
            // We use two different Tweens to animate the same property
            // at different intervals in the animation as required
            // Here the opacity property will listen to the value of the introTextOpacityHide`
            // if the controller's value is more than 0.09 and listen to the value of `introTextOpacityShow` otherwise
            opacity: controller.value > 0.09
                ? introTextOpacityHide
                : introTextOpacityShow,
          ),
          StarWarsLogo(
            opacity: controller.value > 0.2
                ? starWarsLogoOpacityHide
                : starWarsLogoOpacityShow,
            size: starWarsLogoSize,
          ),
          CrawlingText(
            topMargin: crawlingTextPositionTop,
            bottomMargin: crawlingTextPositionBottom,
            opacity: crawlingTextOpacity,
          ),
        ],
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      builder: _buildAnimation,
      animation: controller,
    );
  }
}
```

Below is a visual representation of the Staggered animation timeline. It shows the intervals of all the Tweens in relation to each other and to the total interval of the `AnimationController`.

![Chart showing staggered animation timeline ](https://cdn.hashnode.com/res/hashnode/image/upload/v1612300870979/ZW1shvrFd.png)

**“Chewie, we’re home.” — Han Solo**

Specify the `StarWarsIntro` widget as your `home` and we're set! Once you run the project grab your popcorn 🍿, click on the screen and watch the intro unfold 🎬.

```
void main() {
  runApp(
    MaterialApp(
      title: 'Star Wars Intro',
      home: StarWarsIntro(),
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primarySwatch: Colors.yellow,
      ),
    ),
  );
}
```
Visit [the GitHub repo](https://github.com/thearthurdev/star_wars_intro) to view the complete project.

Visit this [CodePen](https://codepen.io/thearthurdev/pen/YzGoqeV) to see it in action, but the caveat is that there's no music as CodePen doesn't support importing flutter packages yet 😢.

### End Note
Well, that was fun! 

Flutter's animation system is very cool and easy to use once you get the hang of it and from my experience, the only way to get good at it is to jump in feet first, and maybe write about your experience. It helps so solidify your understanding.

You can visit this helpful [doc](https://flutter.dev/docs/development/ui/animations/staggered-animations) by the Flutter team if you need further clarification of Staggered animations. 

I would've had a hard time writing this project and article without [this](https://dev.to/christopherkade/developing-the-star-wars-opening-crawl-in-htmlcss-2j9e) article by Christoper Kade where he does the same thing in HTML/CSS!

**May the force be with you**

\- ArthurDev