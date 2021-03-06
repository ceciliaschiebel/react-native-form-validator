# React native form validator
[![Build Status](https://travis-ci.org/perscrew/react-native-form-validator.svg?branch=master)](https://travis-ci.org/perscrew/react-native-form-validator)

React native form validator is a simple library to validate your form fiels with React Native.
The library is voluntarily easy to use. You juste have to extends the "ValidationComponent" class on your desired React native form component.

## 1. Installation
* Run npm install 'react-native-form-validator' to fetch the library :
```
npm install 'react-native-form-validator' --save
```

## 2. Use it in your app

Extend "ValidationComponent" class on a your form component :
```
import React from 'react';
import ValidationComponent from 'react-native-form-validator';

export default class MyForm extends ValidationComponent {
  ...
}
```
The ValidationComponent extends the React.Component class.

To ensure form validation you have to call the "this.validate" method in a custom function.
```
constructor(props) {
  super(props);
  this.state = {name : "My name", email: "titi@gmail.com", number:"56", date: "2017-03-01"};
}

_onSubmit() {
  // Call ValidationComponent validate method
  this.validate({
    name: {minlength:3, maxlength:7, required: true},
    email: {email: true},
    number: {numbers: true},
    date: {date: 'YYYY-MM-DD'}
  });
}
```
The method arguments should be a representation of the React component state. The first graph level matches with the React state variables.
The second level matches with the existing validation rules.

You will find bellow the default rules available in the library [defaultRules.js](./defaultRules.js) :

|Rule|Benefits|
|-------|--------|
|numbers|Check if a state variable is a number.|
|email|Check if a state variable is an email.|
|required|Check if a state variable is not empty.|
|date|Check if a state variable respects the date pattern. Ex: date: 'YYYY-MM-DD'|
|minlength|Check if a state variable is greater than minlength.|
|maxlength|Check if a state variable is lower than maxlength.|

You can also override this file via the component React props :
```
const rules = {any: /^(.*)$/};

<FormTest rules={rules} />
```


Once you have extended the class a set of usefull methods become avaiblable :

|Method|Output|Benefits|
|-------|--------|--------|
|this.validate(state_rules)|Boolean|This method ensures form validation within the object passed in argument.The object should be a representation of the React component state. The first graph level matches with the React state variables.The second level matches with the existing validation rules.|
|this.isFormValid()|Boolean|This method indicates if the form is valid and if there is no errors.|
|this.isFieldInError(fieldName)|Boolean|This method indicates if a specific field is in error. The field name will matches with your React state|
|this.getErrorMessages(separator)|String|This method retruns the different error messages bound to your React state. The argument is optional, by default the separator is a \n. Under the hood a join method is used.|

The library also contains a [defaultMessages.js](./defaultMessages.js) file wich includes the errors label for a language locale.
You can override this file via the component React props :
```
const messages = {
  en: {numbers: "error on numbers !"},
  fr: {numbers: "erreur sur numbers !"}
};

<FormTest messages={messages} />
```

You can also specify the default custom language locale in the props :

```
<FormTest deviceLocale="fr" />
```


## 3. Complete example

You can find a complete example in the [formTest.js](./test/formTest.js) file :

```
'use strict';

import React, {Component}  from 'react';
import {View, Text, TextInput, TouchableHighlight} from 'react-native';
import ValidationComponent from '../index';

export default class FormTest extends ValidationComponent {

  constructor(props) {
    super(props);
    this.state = {name : "My name", email: "tibtib@gmail.com", number:"56", date: "2017-03-01"};
  }

  _onPressButton() {
    // Call ValidationComponent validate method
    this.validate({
      name: {minlength:3, maxlength:7, required: true},
      email: {email: true},
      number: {numbers: true},
      date: {date: 'YYYY-MM-DD'}
    });
  }

  render() {
      return (
        <View>
          <TextInput ref="name" onChangeText={(name) => this.setState({name})} value={this.state.name} />
          <TextInput ref="email" onChangeText={(email) => this.setState({email})} value={this.state.email} />
          <TextInput ref="number" onChangeText={(number) => this.setState({number})} value={this.state.number} />
          <TextInput ref="date" onChangeText={(date) => this.setState({date})} value={this.state.date} />

          <TouchableHighlight onPress={this._onPressButton}>
            <Text>Submit</Text>
          </TouchableHighlight>

          <Text>
            {this.getErrorMessages()}
          </Text>
        </View>
      );
  }

}
```
