<!--
Components can have properties passed in ([Passing Properties to a Component](../passing-properties-to-a-component/)),
but they can also return output to be used in a block expression.
-->

コンポーネントはプロパティを渡すことができます([コンポーネントへのプロパティの受け渡し](../passing-properties-to-a-component/))が、ブロック式で使用する出力を返すこともできます。

<!--
### Return values from a component with `yield`
-->

### `yield`を持つコンポーネントからの戻り値

```app/templates/index.hbs
{{blog-post post=model}}
```

```app/templates/components/blog-post.hbs
{{yield post.title post.body post.author}}
```

<!--
Here an entire blog post model is being passed to the component as a single component property.
In turn the component is returning values using `yield`.
In this case the yielded values are pulled from the post being passed in
but anything that the component has access to can be yielded, such as an internal property or something from a service.
-->

ここでは、ブログポストモデル全体が単一のコンポーネントプロパティとしてコンポーネントに渡されています。
次にコンポーネントは`yield`を使用して値を返します。
この場合、得られた値は渡されたポストから取り出されますが、内部プロパティやサービスからの何かなど、コンポーネントがアクセスできるものが生成できます。

<!--
### Consuming yielded values with block params
-->

### ブロックパラメータを使用して消費された値

<!--
The block expression can then use block params to bind names to any yielded values for use in the block.
This allows for template customization when using a component,
where the markup is provided by the consuming template,
but any event handling behavior implemented in the component is retained such as `click()` handlers.
-->

ブロック式では、ブロック・パラメーターを使用して、名前をブロック内で使用するために値をバインドすることができます。
これにより、消費テンプレートによってマークアップが提供されるコンポーネントを使用するときにテンプレートのカスタマイズが可能になりますが、`click()`ハンドラなどのコンポーネントで実装されたイベント処理動作はすべて保持されます。

```app/templates/index.hbs
{{#blog-post post=model as |title body author|}}
  <h2>{{title}}</h2>
  <p class="author">by {{author}}</p>
  <p class="post-body">{{body}}</p>
{{/blog-post}}
```

<!--
The names are bound in the order that they are passed to `yield` in the component template.
-->

名前は、コンポーネントテンプレートで`yeild`に渡される順序でバインドされます。

<!--
### Supporting both block and non-block component usage in one template
-->

### 1つのテンプレートでブロックコンポーネントと非ブロックコンポーネントの両方の使用をサポート

<!--
It is possible to support both block and non-block usage of a component from a single component template
using the `has-block` helper.
-->

`has-block`ヘルパーを使用して、単一のコンポーネントテンプレートからコンポーネントのブロック使用と非ブロック使用の両方をサポートすることができます。

```app/templates/components/blog-post.hbs
{{#if (has-block)}}
  {{yield post.title post.body post.author}}  
{{else}}
  <h1>{{post.title}}</h1>
  <p class="author">Authored by {{post.author}}</p>
  <p>{{post.body}}</p>
{{/if}}
```

<!--
This has the effect of providing a default template when using a component in the non-block form
but providing yielded values for use with block params when using a block expression.
-->

これは、非ブロック形式でコンポーネントを使用するときに、ブロック式を使用するときにブロックパラメータとともに使用するために得られた値を提供するときに、デフォルトのテンプレートを提供するという効果があります。
