# 作为Web开发人员应避免的5种CSS做法!

有人认为CSS很难学习，没有什么逻辑可言，而且还坑很多，可能是大家对CSS还不是很了解，因为我提出了五个我不喜欢的开发者习惯，并向大家展示如何避免它们。

## 1.设置边距或填充，然后将其重置

- 我经常看到有人为所有元素设置边距或填充，然后又将第一个或最后一个元素的值重置。

```css
.item {
  margin-right: 1.6rem;
}

.item:last-child {
  margin-right: 0;
}
```

- 对于更简单，更简洁的CSS，请使用以下选项之一：nth-child / nth-of-type选择器，：not（）伪类或相邻的兄弟组合器，而后者又被称为+。

```css
.item:not(:last-child) {
  margin-right: 1.6rem;
}
```
```css
.item:nth-child(n+2) {
  margin-left: 1.6rem;
}
```
```css
.item + .item {
  margin-left: 1.6rem;
}
```

## 2 为position:absolute/fixed的元素添加display:block 

```css
.button::before {
  content: "";
  position: absolute;
  display: block;
}
```
```css
.button::before {
  content: "";
  position: fixed;
  display: block;
}
```

- 你知道吗？当你为一个element设置`position: absolute` 或 `position: fixed`的时候，默认就有`display: block`这个属性！

- 因此，在这种情况下，如果你设置`display:inline-*`的时候，将会做如下的转换`inline`或`inline-block`将转换成`block`，`inline-flex`转换成`flex`，`inlone-grid`转换成`grid`，`inline-table`转换成`table`

我们可以改成这样：

```css
.button::before {
  content: "";
  position: absolute;
}
```

```css
.button::before {
  content: "";
  position: flxed;
}
```

## 3 使用transform: translate (-50%, -50%)居中

- 让一个element水平垂直居中，其中的一种解决方案是结合使用`position: absolute`和使用`transform`属性。但是，这个技术在基于Chromium的浏览器中引起了模糊的文本问题。
```css
.parent {
  position: relative;
}

.child {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```
- 在引入flexbox之后，我认为该技术已不再适用。 问题是它不能解决文本模糊的问题。
- 而且，它使用了五个属性。 因此，我想分享一个技巧，可以将代码减少为两个属性。
- 我们可以使用`margin:auto`在flex容器内自动放置，浏览器将元素居中。 只有两个属性，仅此而已。

```css
.parent {
  display: flex;
}

.child {
  margin: auto;
}
```

### 4.在块级元素上设置 width: 100%

- 我们经常使用flexbox创建一个多列网格，该网格逐渐转换为单列。

- 为了将网格转换为一列，开发人员使用宽度：100％。我不明白他们为什么这么做。网格元素是块元素，默认情况下可以执行此操作，而无需使用其他属性。

```html
<div class="parent">
  <div class="child">Item 1</div>
  <div class="child">Item 2</div>
  <div class="child">Item 3</div>
  <div class="child">Item 4</div>
</div>
```
```css
.parent {
  display: flex;
  flex-wrap: wrap;
}

.child {
  width: 100%;
}

@media (min-width: 1024px) {
  .child {
    width: 25%;
  }
}
```

- 因此，我们不需要使用width：100％，而是应该编写媒体查询，以便flexbox仅用于创建多列网格。
```css
@media (min-width: 1024px) {
  .parent {
    display: flex;
    flex-wrap: wrap;
  }

  .child {
    width: 25%;
  }
}
```
### 5. 为Flex布局下的子元素，设置display: block


```css
.parent {
  display: flex;
}

.child {
  display: block;
}
```

- 使用flexbox时，请务必记住，创建Flex容器（添加显示：flex）时，所有子项（flex项）都会被阻塞。
- 这意味着子元素被设置为`display：block`，并且只能具有`block`。因此，如果你设置了`inline`或`inline-block`，它将更改为`block`，`inline-flex`=> `flex`，`inline-grid`=> `grid`和`inline-table`=> `table`。
- 因此，不需要为flex的子项元素添加`display：block`，浏览器默认就会添加。



