<!--
You can respond to user events on your component like double-clicking, hovering,
and key presses through event handlers. Simply implement the name of the event
you want to respond to as a method on your component.
-->

イベントハンドラを使用して、ダブルクリック、ホバーリング、キー押下など、コンポーネントのユーザーイベントに応答することができます。
コンポーネントのメソッドとして応答したいイベントの名前を実装するだけです。

<!--
For example, imagine we have a template like this:
-->

たとえば、次のようなテンプレートがあるとします。

```hbs
{{#double-clickable}}
  This is a double clickable area!
{{/double-clickable}}
```

<!--
Let's implement `double-clickable` such that when it is
clicked, an alert is displayed:
-->

クリックすると警告が表示されるようにダブルクリックを実装しましょう。

```app/components/double-clickable.js
import Component from '@ember/component';

export default Component.extend({
  doubleClick() {
    alert("DoubleClickableComponent was clicked!");
  }
});
```

<!--
Browser events may bubble up the DOM which potentially target parent component(s)
in succession. To enable bubbling `return true;` from the event handler method
in your component.
-->

ブラウザイベントは、潜在的に親コンポーネントを連続して対象とするDOMをバブルアップさせる可能性があります。
バブリングを有効にするには、コンポーネントのイベントハンドラメソッドで`return true;`します。

```app/components/double-clickable.js
import Component from '@ember/component';

export default Component.extend({
  doubleClick() {
    Ember.Logger.info("DoubleClickableComponent was clicked!");
    return true;
  }
});
```

<!--
See the list of event names at the end of this page. Any event can be defined
as an event handler in your component.
-->

このページの最後にあるイベント名のリストを参照してください。 どのイベントも、コンポーネント内のイベントハンドラとして定義できます。

<!--
## Sending Actions
-->

## アクションの送信

<!--
In some cases your component needs to define event handlers, perhaps to support
various draggable behaviors. For example, a component may need to send an `id`
when it receives a drop event:
-->

場合によっては、コンポーネントがさまざまなドラッグ可能な動作をサポートするために、イベントハンドラを定義する必要があります。
たとえば、コンポーネントは、ドロップイベントを受け取ったときに`id`を送信する必要があります。

```hbs
{{drop-target action=(action "didDrop")}}
```

<!--
You can define the component's event handlers to manage the drop event.
And if you need to, you may also stop events from bubbling, by using
`return false;`.
-->

コンポーネントのイベントハンドラを定義して、ドロップイベントを管理できます。
また、必要に応じて、 `return false;`を使用して、バブリングからイベントを停止することもできます。

```app/components/drop-target.js
import Component from '@ember/component';

export default Component.extend({
  attributeBindings: ['draggable'],
  draggable: 'true',

  dragOver() {
    return false;
  },

  drop(event) {
    let id = event.dataTransfer.getData('text/data');
    this.get('action')(id);
  }
});
```

<!--
In the above component, `didDrop` is the `action` passed in. This action is
called from the `drop` event handler and passes one argument to the action -
the `id` value found through the `drop` event object.
-->

上記のコンポーネントでは、`didDrop`は渡された`action`です。
このアクションは、`drop`イベントハンドラから呼び出され、1つの引数、つまり`drop`イベントオブジェクトを通じて見つかった`id`値をアクションに渡します。

<!--
Another way to preserve native event behaviors and use an action, is to
assign a (closure) action to an inline event handler. Consider the
template below which includes an `onclick` handler on a `button` element:
-->

ネイティブイベントビヘイビアを保持し、アクションを使用する別の方法は、インラインイベントハンドラに(クロージャ)アクションを割り当てることです。
下記のテンプレートを考えてみましょう。`button`要素に`onclick`ハンドラが含まれています：

```hbs
<button onclick={{action 'signUp'}}>Sign Up</button>
```

<!--
The `signUp` action is simply a function defined on the `actions` hash
of a component. Since the action is assigned to an inline handler, the
function definition can define the event object as its first parameter.
-->

`signUp`アクションは、単にコンポーネントの`actions`ハッシュで定義された関数です。
アクションはインラインハンドラに割り当てられているため、関数定義では最初のパラメータとしてイベントオブジェクトを定義できます。

```js
actions: {
  signUp(event){
  	// Only when assigning the action to an inline handler, the event object
    // is passed to the action as the first parameter.
  }
}
```

<!--
The normal behavior for a function defined in `actions` does not receive the
browser event as an argument. So, the function definition for the action cannot
define an event parameter. The following example demonstrates the
default behavior using an action.
-->

`actions`で定義された関数の通常の動作は、ブラウザイベントを引数として受け取らない。
したがって、アクションの関数定義ではイベントパラメーターを定義できません。 次の例は、アクションを使用したデフォルトの動作を示しています。

```hbs
<button {{action 'signUp'}}>Sign Up</button>
```

```js
actions: {
  signUp() {
    // No event object is passed to the action.
  }
}
```

<!--
To utilize an `event` object as a function parameter:
-->

`event`オブジェクトを関数パラメータとして使用するには、

<!--
- Define the event handler in the component (which is designed to receive the
  browser event object).
- Or, assign an action to an inline event handler in the template (which
  creates a closure action and does receive the event object as an argument).
-->

- コンポーネント(ブラウザイベントオブジェクトを受け取るように設計されている)にイベントハンドラを定義します。
- または、テンプレート内のインラインイベントハンドラにアクションを割り当てます(クロージャアクションを作成し、イベントオブジェクトを引数として受け取ります)。


<!--
## Event Names
-->

## イベント名

<!--
The event handling examples described above respond to one set of events.
The names of the built-in events are listed below. Custom events can be
registered by using [Application.customEvents](https://www.emberjs.com/api/ember/2.16/classes/Application/properties/customEvents?anchor=customEvents).
-->

上記のイベント処理の例は、1組のイベントに応答します。
組み込みイベントの名前は次のとおりです。
カスタムイベントは、[Application.customEvents](https://www.emberjs.com/api/ember/2.16/classes/Application/properties/customEvents?anchor=customEvents)を使用して登録できます。

<!--
Touch events:
-->

タッチイベント:

* `touchStart`
* `touchMove`
* `touchEnd`
* `touchCancel`

<!--
Keyboard events
-->

キーボードイベント:

* `keyDown`
* `keyUp`
* `keyPress`

<!--
Mouse events
-->

マウスイベント:

* `mouseDown`
* `mouseUp`
* `contextMenu`
* `click`
* `doubleClick`
* `mouseMove`
* `focusIn`
* `focusOut`
* `mouseEnter`
* `mouseLeave`

<!--
Form events:
-->

フォームイベント:

* `submit`
* `change`
* `focusIn`
* `focusOut`
* `input`

<!--
HTML5 drag and drop events:
-->

HTML5ドラッグ&ドロップイベント:

* `dragStart`
* `drag`
* `dragEnter`
* `dragLeave`
* `dragOver`
* `dragEnd`
* `drop`
