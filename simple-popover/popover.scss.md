

###### A simple popover - style


```

.custom-popover {
  @include card-base;
  position: absolute;
  display: inline-block;
  right: 17px;
  width: 400px;
  top: -10px;

  h5 {
    text-transform: none;
    color: $color-brand;
    margin-bottom: 0;
    font-size: 1rem;
  }

  p {
    margin-top: 0;
    margin-bottom: 10px;
    text-transform: none;
  }

  &::after,
  &::before {
    left: 100%;
    top: 9px;
    border: solid transparent;
    content: '';
    height: 0;
    width: 0;
    position: absolute;
    pointer-events: none;
  }

  &::after {
    border-color: rgba(111, 213, 179, 0);
    border-left-color: white;
    border-width: 5px;
    margin-top: -5px;
  }
  &::before {
    border-color: rgba(235, 235, 235, 0);
    border-left-color: $color-light-gray;
    border-width: 8px;
    margin-top: -8px;
  }
}
```