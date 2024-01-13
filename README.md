#  How to make your Flutter app accessible -  2021.12.08.
<p>
  <a href="https://github.com/gerfalcon">
    <img alt="GitHub: gerfalcon" src="https://img.shields.io/github/followers/gerfalcon?label=Follow&style=social" target="_blank" />
  </a>
  <a href="https://twitter.com/SandroMaglione">
    <img alt="Twitter: gerfalcon" src="https://img.shields.io/twitter/follow/GerfalconVogel?style=social" target="_blank" />
  </a>
</p>


A Flutter project demonstrating support for localization and accessibility features and used for the following [How to make your Flutter app accessible - 12/2021](https://youtu.be/VGfzT_AuVPI?t=3240) talk I gave at the December 2021 HWSW free! Meetup online event in Hungarian.
- Slides are available [here](https://speakerdeck.com/gerfalcon/how-to-make-your-flutter-app-accessible).
- Recording on [YouTube](https://youtu.be/VmB_KLk28f8?t=5)
Impact: 1250+ online views


<p>
<img src="https://github.com/gerfalcon/gerfalcon/assets/15221068/b6dcbfc9-8a6f-4cd5-9392-86ea8234e9c4" width="400"/>
</p>

## Sample app
<p align="left">
<img src="https://user-images.githubusercontent.com/15221068/145232602-2d2a1fd0-adba-48c1-9c76-6a1cd018a3ce.gif" alt="drawing" width="300"/>
</p>


With any luck, hundreds, thousands (or millions!) could use our apps after publishing them to the app stores. Apps supporting multiple languages and accessibility features can appeal to a broader audience worldwide.

#### Key abbreviations:
- `a11y`: accessibility
- `i18n`: internationalization
- `l10n`: localization

## Accessibility
In a nutshell, accessibility means that as many people can use our mobile app as possible, including those with disabilities, like visual or motor impairment.

### Internationalization and localization
If we want to deploy apps to users who speak a different language than our default one, having apps support the target audience's native language and culture can be appealing in those markets. Internationalization means we should develop an app in a way that makes it possible to localize values like text and layouts for each language the app supports.
#### Built-in solution
By default, Flutter only supports US English localization. To add support for other languages, the Flutter team recommends using a package called [flutter_localizations](https://api.flutter.dev/flutter/flutter_localizations/flutter_localizations-library.html).
To use flutter_localizations, we have to add it to our `pubspec.yaml` file:

```
dependencies:
  flutter:
    sdk: flutter
  flutter_localizations:
    sdk: flutter 
```

After a successfully completed `pub get packages` command, we have to specify additional list type properties for `MaterialApp`:
- `localizationsDelegates[]`,
- `supportedLocales[]`.

Next, we have to add predefined global values (`GlobalMaterialLocalizations.delegate`, `const Locale(’hu’)`) to make the application's widgets adapt to the current language.

Besides the  `flutter_localizations` package, we can add the [intl](https://pub.dev/packages/intl) package too, which provides internationalization and localization facilities, including message translation, plurals and genders, date/number formatting and parsing, and bidirectional text support. Let's include this package too in the project's `pubspec.yaml` file:
```
dependencies:
  intl: ^0.17.0
```
Additionally, we should enable the generate flag in the `pubspec.yaml` file:
```
flutter:  
	generate: true
```
This flag is essential to generate localizations. To configure where these files are generated, we can add a new `l10n.yaml` file to the root directory of the project with the following content:

```
arb-dir: lib/l10n  
template-arb-file: app_en.arb  
output-localization-file: l10n.dart  
output-class: L10n
```
With this file we can specify the following settings:
-   The input files (`app_en.arb` and `app_hu.arb`) will be located in `${FLUTTER_PROJECT}/lib/l10n`,
-   The `app_en.arb` input file will provide the template,
-   The generated localizations will be placed in the `l10n.dart` file

In the generated `l10n.dart` file, the class that we'll need is named `L10n`.

We can add the default English localization to the `app_en.arb` (*ARB* format  = *Application Resource Bundle*) template file. For example, to store the localization of our home screen's title we can add the following key-value pairs:
```
{
  "homeTitle": "Know your Santa",
  "@homeTitle": {
    "description": "Page title.",
    "context": "HomePage"
  }
}
```

The `homeTitle` is the *resource ID* and its value is the localization string (“*Know your Santa*”).
Because the `app_en.arb` file is the template file, we have to add an attribute item keyed by the original resource ID plus a prefix ‘`@`’ character. In the attribute item **optional** we can use predefined resource attributes like:
-   **`description`**: A short paragraph describing the resource and how it is being used by the app, and messages that need to be passed to the localization process and translators. Every attribute item must have a `description` as a requirement.
- **`context`**: It describes the context in which this resource applies. When this piece of information is missing, it defaults to global `context`.

In the same directory, let's create the `app_hu.arb` file for Hungarian translations and add a translation for the title string:
```
{
  "homeTitle": "Ismerd meg a Mikulásod"  
}
```

Here's the complete `app_en.arb` file for the example project:
```
{
  "@@locale": "en",
  "homeTitle": "Know your Santa",
  "@homeTitle": {
    "description": "Page title.",
    "context": "HomePage"
  },
  "homeAppbarTitle": "HomePage",
  "@homeAppbarTitle": {
    "description": "Page title of appbar.",
    "context": "HomePage"
  },
  "homeCurrentLanguage": "Current language: {language}",
  "@homeCurrentLanguage": {
    "description": "Show the current language on the Home page",
    "context": "HomePage",
    "placeholders": {
      "language": {}
    }
  },
  "homeYourSanta": "Your santa",
  "@homeYourSanta": {
    "description": ""
  },
  "santaName": "Santa Claus",
  "@santaName": {
    "description": ""
  },
  "santaHat": "Hat",
  "@santaOutfit1": {
    "description": ""
  },
  "santaTie": "Tie",
  "@santaOutfit2": {
    "description": ""
  },
  "santaDescription": "Jingle bells jingle bells\nJingle all the way\nOh what fun it is to ride\nIn a one horse open sleigh\nJingle bells jingle bells\nJingle all the way\nOh what fun it is to ride\nIn a one horse open sleigh\n\nDashing through the snow\nIn a one horse open sleigh\nOver the fields we go\nLaughing all the way\nBells on bob tail ring\nMaking spirits bright\nOh What fun it is to ride \nA sleighing song tonight\n\nOh, oh jingle bells jingle bells\nJingle all the way\nOh what fun it is to ride\nIn a one horse open sleigh\n\nYou better not pout\nYou better not cry\nYou better not shout\nI'm telling you why\nSanta Claus is coming to town\n\nHe's making a list\nHe's checking it twice\nGonna find out\nWho's naughty or nice\nSanta Claus is coming to town\n\nHe sees you when you're sleeping\nHe knows if you're awake\nHe knows if you've been bad or good\nSo be good for goodness sake\n\nI am dreaming of a white Christmas\nJust like the ones I used to know\nWhere the tree tops glisten\nAnd children listen\nTo hear sleighbells in the snow\nI am dreaming of a white Christmas\nWith every Christmas card I write\nMay your days be merry and bright\nAnd may all your Christmases be\n\nOh, the weather outside is frightful\nBut the fire is so delightful\nAnd since we've got no place to go\nLet it snow, let it snow, let it snow\n\nOh, it doesn't show signs of stopping\nAnd I've brought some corn for popping\nThe lights are turned way down low\nLet it snow, let it snow, let it snow\n\nWhen we finally kiss goodnight\nHow I'll hate going out in the storm\nBut, if you really hold me tight\nAll the way home I'll be warm\n\nThe fire is slowly dying\nAnd my dear, we're still good by ing\nBut, as long as you love me so\nLet it snow, let it snow, let it snow\n\nHere comes Santa Claus\nHere comes Santa Claus\nRight down Santa Claus Lane\nVixen and Blitzen and all his reindeer\nPullin' on the reins\nBells are ringin', children singin'\nAll is merry and bright\nHang your stockings and say your prayers\nSanta Claus comes tonight\n\nHere comes Santa Claus\nHere comes Santa Claus\nRight down Santa Claus Lane\nHe's got a bag that's filled with toys\nFor boys and girls again\nHear those sleigh bells jingle jangle\nOh what a beautiful sight\nJump in bed, and cover your head\nSanta Claus comes tonight\nSanta Claus comes tonight",
  "@santaDescription": {
    "description": ""
  }
}
```


And this is the complete `app_hu.arb` file:
```
{
  "@@locale": "hu",
  "homeTitle": "Ismerd meg a Mikulásod",
  "homeAppbarTitle": "Kezdőlap",
  "homeCurrentLanguage": "Aktuális nyelv: {language}",
  "homeYourSanta": "A mikulásod",
  "santaName": "Mikulás",
  "santaHat": "Kalap",
  "santaTie": "Nyaktekerészeti mellfekvenc",
  "santaDescription": "Hull a pelyhes fehér hó,\njöjj el kedves télapó.\nMinden gyermek várva vár,\nvidám ének hangja száll.\nVan zsákodban minden jó,\npiros alma, mogyoró.\nJöjj el hozzánk, várunk rád,\nkedves öreg télapó!\n\nNagyszakállú télapó,\njó gyermek barátja.\nCukrot, diót, mogyorót\nrejteget a zsákja.\nAmerre jár reggelig,\nkis cipőcske megtelik.\nMegtölti a télapó,\nha üresen látja."
}
```

Whenever we run the application the code generation takes place. We can see the generated files under `${FLUTTER_PROJECT}/.dart_tool/flutter_gen/gen_l10n`:

-   `l10n.dart`, 
-   `l10n_en.dart`,
-   `l10n_hu.dart`.
      
In the `main.dart` file `MaterialApp` needs to include `L10n.delegate()` in the `localisationDelegates` list, and the locales we support in the `supportedLocales` list:

``` dart
import 'package:flutter/material.dart';
import 'gen_l10n/l10n.dart';

import 'presentation/page/home/home_page.dart';

class App extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter l10n demo',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primaryColor: Colors.red,
      ),
      localizationsDelegates: L10n.localizationsDelegates,
      supportedLocales: L10n.supportedLocales,
      home: HomePage(),
    );
  }
}
```

Callers can look up localized strings using an instance of the `L10n` class returned by `L10n.of(context)`. For instance, in the `home_page.dart` file at the top of the `build()` method of the `HomePage` `Widget` we can get a reference to the `L10n` object and pass its `homeTitle` value to the title text:

``` dart
import 'package:flutter_gen/gen_l10n/l10n.dart';
/// (...)

class HomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final L10n l10n = L10n.of(context);
    return Scaffold(
      /// (...)
      body: Container(
        width: double.infinity,
        padding: EdgeInsets.symmetric(horizontal: 16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.center,
          children: [
            /// (...)
            Text(l10n.homeTitle),
            /// (...)
          ],
        ),
      ),
    );
  }
}
```

As a result, users can see the home page according to the system language of their device:
en| hu 
:-: | :-:
<img src="https://user-images.githubusercontent.com/15221068/117217332-87b63e00-ae01-11eb-9ef5-fac34c7a3648.png" height="406"> | <img src="https://user-images.githubusercontent.com/15221068/117216778-8e908100-ae00-11eb-9ecf-7b75e7fbaeaf.png" height="406">


### RTL support
In some languages, for instance, in Arabic writing goes from the right to the left (RTL) instead of from the left to the right like in English or Hungarian. Localization for RTL languages needs the text direction to be changed.
To demonstrate RTL support we can add and `.arb` file for Arabic string resources and we use the `start` and `end` properties of the `EdgeInsetsDirectional` class instead of the basic `left` and `right` of the `EdgeInsets` class.

![image](https://user-images.githubusercontent.com/15221068/134827523-69c41179-e61a-45ce-b3f9-87503909d108.png)
### Text-to-Speech 
People with visual disabilities often use the default [TalkBack](https://support.google.com/accessibility/android/answer/6283677?hl=en) (Android) or [VoiceOver](https://www.apple.com/lae/accessibility/vision/) (iOS) assistants to read the content of screens for them.
If we want to specify exactly what will these assistants say about our apps' screens, we can do that using the [Semantics](https://api.flutter.dev/flutter/widgets/Semantics-class.html) widget which has tons of properties regarding such settings. It can be used for analytics too. 

For example, we can wrap one of our `Text` widgets into a `Semantics` widget and we can add a `label` property.
``` dart
title: Semantics(
      child: Text(l10n.homeAppbarTitle),
      label: "This is the HomePage.",
    ),
```

- [FIGMA LINK](https://www.figma.com/file/DaVkJeksKxggpWD3lOAA07/Know-your-Santa?node-id=0%3A1)

## Further reading, materials

- [Internationalizing Flutter apps](https://flutter.dev/docs/development/accessibility-and-localization/internationalization) official documentation by the Flutter team
- [Observable Flutter #31: Accessibility episode](https://www.youtube.com/watch?v=j7mWmOiLIkI&t=11s&ab_channel=Flutter) where Craig Labenz is joined by Manuela Sakura Rommel to discuss advanced accessibility tips and tricks
- [Stop treating accessibility as an afterthought: Concrete steps to build inclusive apps](https://www.droidcon.com/2023/11/15/stop-treating-accessibility-as-an-afterthought-concrete-steps-to-build-inclusive-apps-2/) talk by Manuela Sakura Rommel at [fluttercon](https://fluttercon.dev/)
- Used icon by [Icons8](https://icons8.com/vector-creator/illustration/60799f58487a400015ee9ac9)
