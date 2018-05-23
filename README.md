# Mobile Flashcards Project

The objective of the project is to build mobile application (Android or iOS or both) that allows users to study collections of flashcards. 

The app will have the following functionality:

1. list of flashcards created by user which are grouped by different categories in "decks".
2. User could create new deck by entering a title.
3. Inside the individual deck, the user can add flashcard or "card" to the existing deck.
4. When adding a new card, the user will enter the question, create a list of choices for the question as well as the correct choice as the answer.
5. The user can test themselve by doing a quiz on the cards in a deck.
6. The users are do at least a quiz a day. A notification will appear after midday (NZ time), remind the user to do a quiz. Once a quiz is done, the notifcation on that day will not be appear. 


## How to run this project

We need to install all project dependencies and start the development server, run the following commands :

* `yarn install`
* `yarn start`

I have tested the app under following environment:

1. Genymotion emulator - Samsung Galaxy S6 - 5.1.0 - API 22 - 1440x2560
2. Actual phone - Motorola Moto G - 5.1 - Modal XT1033 - 720 x 1280
3. Apple iPad Generation 2 


## Project configuration

This project was first bootstrapped with [Create React Native App](https://github.com/react-community/create-react-native-app)

I have make some modification to the project file structure, starting from the following file:

### package.json

```bash
{
  "name": "mobile-flashcards",
  "version": "0.8.0",
  "private": true,
  "devDependencies": {
    "babel-plugin-module-resolver": "^3.1.1",
    "jest-expo": "~27.0.0",
    "react-native-scripts": "1.14.0",
    "react-test-renderer": "16.3.1"
  },
  "main": "./src/index.js",
  "scripts": {
    "start": "react-native-scripts start",
    "eject": "react-native-scripts eject",
    "android": "react-native-scripts android",
    "ios": "react-native-scripts ios",
    "test": "jest"
  },
  "jest": {
    "preset": "jest-expo"
  },
  "dependencies": {
    "expo": "^27.0.1",
    "react": "16.3.1",
    "react-native": "~0.55.2",
    "react-navigation": "^2.0.1",
    "react-redux": "^5.0.7",
    "redux": "^4.0.0",
    "redux-promise-middleware": "^5.1.1"
  }
}
```
As you can see, I have move all javaScript code into `src` directory and have changed the initial start file to `./src/index.js`, which will run `App` component in `./components`.

### ./src/index.js

```bash
import Expo from 'expo';
import App from './components/App';
import React, { Component } from 'react';
import { View } from 'react-native';

if (process.env.NODE_ENV === 'development') {
  Expo.KeepAwake.activate();
}

Expo.registerRootComponent(App);
```

Also I have also added [babel-plugin-module-resolver](https://www.npmjs.com/package/babel-plugin-rn-module-resolver) package to use alias for directories, specific files, or even other npm modules instead of using relative paths in the project.

To do so, following file has been ammended:

### .babelrc

```bash
{
  "presets": ["babel-preset-expo"],
  "env": {
    "development": {
      "plugins": [
        "transform-react-jsx-source",
        [ "module-resolver", {
          "root": ["./src"],
          "extensions": [".js", ".ios.js", ".android.js"] 
          } ]        
      ]
    }
  }
}
```
## Project structure
```bash
├── README.md - This file.
├── package.json # npm package manager file.
└── src
    ├── actions # contains Action creator files
    │   ├── deck.js # Action creator for deck.
    │   ├── decks.js # Action creators for decks or flashcards.
    │   ├── notice.js # Action creators for notification.
    ├── components
    │   ├── AddQuestion # components related to 'Add Question' funtionality.  
    │   │   ├── AddQuestionView.js # the main components for 'Add Question' screen. It handles all the state as well as provide functionality for the other components.
    │   │   ├── AnswerSelectView.js # this component provide a Picker for user to select a 'Choice' as the answer to the 'Question' as well as  a 'Submit' button to save the 'Question to the 'Deck' collection.
    │   │   ├── MultiChoiceView.js # this component provide a Text Input field for user to keyin 'Choice' for the question. There is a Flat List component which will show the list of 'Choice' for the 'Question'.
    │   │   └── QuestionView.js # This component proide a Text Input field for user to enter text for the 'Question'.
    │   ├── Common # The folder contain reuseable 'Common' component used by other components
    │   │   └── TextButton.js  # This component proide 'Common Button' component with animated effect when pressed as well as provide a text label on the 'Button'. This component will be 'disable' mode until a valid data validation is done and a flag will be set to 'enable' the component.
    │   ├── CreateDeck # components related to 'Create Deck' funtionality.
    │   │   └── CreateDeckView.js  # This component proide a Input Text field for user to enter the title for the new Deck, as well as a 'Create Deck' button to save the new deck.
    │   ├── Deck # components related to 'Deck' detail funtionality.
    │   │   └── DeckView.js  # This component display the detail information about the deck. It also provide 2 buttons : 'Create New Question' button for user to navigate to 'Add Question' Screen (AddQuestionView.js). 'Start Quiz' button for user to navigate to 'Quiz' Screen (QuizView.js).
    │   ├── DeckList # components related to 'Deck Lis' funtionality.
    │   │   ├── DeckListItem.js  # This compoent display a 'Deck' information in Touchable component which show animation when pressed and when pressed, the app will navigate the user to 'Deck' detail screen (DeckView.js).
    │   ├── Navigator # components related to Navigation funtionality.
    │   │   ├── DeckLTabsNavigator.js  # This components provide the 'Tab' navigation functionality. It will provide 2 tabs. The first will display 'Deck List' SCreen (DeckListView.js), and the second will show 'Create Deck' Screen. (CreateDeck.js)
    │   │   └── MainNavigator.js  # This component provide the 'Stack' navigation. It will first house the DeckNavigator component, which in turn will show 'Deck List' screen first and from the 'Deck List' screen, user can navigate to other screen.
    │   ├── Quiz # components related to 'Quiz' funtionality.
    │   │   ├── AnswerView.js  # This component provide the 'Answer' Screen for the 'Quiz'. It will provide the question as well as the answer for the auestion. It will also tell the user whether he/she has answer the question. It has 2 mode: the first mode, if there is more question, a 'Next Question' button will appear for user to move to the next question. the second mode will show when the app comes to the last question fo the quiz, it will show the user how many question they has answered correctly and also provide additional 2 buttons : 'Restart Quiz' button to restart the quiz. 'Back to Deck' button to navigate user back to 'Deck' Screen. (DeckView.js)
    │   │   ├── QuestionHeader.js # This component displays the quiz qestion.
    │   │   ├── QuestionBody.js # This component displays a mutiple choices answer for the question for user to choose.
    │   │   ├── QuestionHeader.js # This component provides a Picker component for user to select a choice as well as a 'Show Answer' button to move to the 'Answer' screen.
    │   └── App.js # This component initialize store, the middleware and then call MainNavigator component.
    ├── contants # contains files with constant declaration, which will be used by other react component.
    │   ├── database.constants.js # contains storage key for flashcard and notification. Also contain data pending, fulfilled and reject constant.
    │   ├── deck.constants.js # contains constants used by deck reducer as well as Redux Promise Middleware when fetching deck information from AsyncStorage.
    │   └── decks.constants.js # contains constants used by decks/flashcards reducer as well as Redux Promise Middleware when fetching decks/flashcards information from AsyncStorage. 
    │   └── notice.constants.js # contains constants used by notification reducer as well as Redux Promise Middleware when fetching notification information from AsyncStorage. 
    ├── helpers # contains files related to AsyncStorage operation.
    │   ├── database.js # contains all API for processing AysncStorage operation. 
    │   └── flashcards.js # contains sample flashcards.
    └── reducers # components that specify how the application's state changes in response to actions sent to the store.
    │   ├── deck.js # reducers for deck Action.
    │   ├── decks.js # reducers for decks Action.
    │   ├── index.js # main file that exports all reducers.
    │   └── notice.js # reducers for notification Action.
    └── index.js # This file is the main entry point for the app.
```

## Following are rescources that I used in preparing for this project

* revise all react native lessons in udacity nanodegree.
* [How to handle side effects or asynchronous actions in a redux application](https://medium.com/react-native-training/redux-4-ways-95a130da0cdc)
* [Android stack header too high](https://github.com/react-navigation/react-navigation/issues/12)
* [Quick Tip: How to Sort an Array of Objects in JavaScript](https://www.sitepoint.com/sort-an-array-of-objects-in-javascript/)
* [How to shuffle an array in JavaScript](https://www.frankmitchell.org/2015/01/fisher-yates/)