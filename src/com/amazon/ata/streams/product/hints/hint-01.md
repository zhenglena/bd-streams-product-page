## What does `<variable> = <condition> ? <value1> : <value2>` do?

This is called the ternary operator, and it acts similar to an if-then-else block.

The statement:
```
<variable> = <condition> ? <value1> : <value2>
```

is functionally similar to:
```
if (<condition>) {
    <variable> = <value1>;
} else {
    <variable> =  <value2>;
}
```

For example, let's say we have the following methods:

```
public Builder upscaleToWidth(final int scaledWidth) {
    if (scaledWidth <= 0) {
        throw new IllegalArgumentException("Cannot scale image to 0 pixels or less, got: " + scaledWidth);
    }
    this.height = getScaledHeight(scaledWidth, width, height);
    this.width = scaledWidth;
    rendering.append("_UX")
        .append(scaledWidth);
    return this;
}

private int getScaledHeight(final int scaledWidth, final int width, final int height) {
  if (width > 0) {
    return scaledWidth * this.height / this.width;
  } else {
    return 0
  }
}
```

Using a ternary operator, you can reduce this code to the following:

```
public Builder upscaleToWidth(final int scaledWidth) {
    if (scaledWidth <= 0) {
        throw new IllegalArgumentException("Cannot scale image to 0 pixels or less, got: " + scaledWidth);
    }
    this.height = this.width > 0 ? scaledWidth * this.height / this.width : 0;
    this.width = scaledWidth;
    rendering.append("_UX")
        .append(scaledWidth);
    return this;
}
```

You can read more about ternary operators in [this JDK8 tutorial](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op2.html)
under the section titled "The Conditional Operators".
