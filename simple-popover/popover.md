
###### A simple popover


```jsx

import React, { useState } from 'react';

const Popover = ({ icon, items, cssClass }) => {
  const [popoverOpen, setPopoverOpen] = useState(false);

  const onHover = () => {
    setPopoverOpen(true);
  };

  const onHoverLeave = () => {
    setPopoverOpen(false);
  };

  const defaultIconInfo = 'ⓘ';

  return (
    <div>
      <i onMouseEnter={onHover} onMouseLeave={onHoverLeave}>
        {icon ? icon : defaultIconInfo}
      </i>
      {popoverOpen && (
        <div className={`custom-popover ${cssClass ? cssClass : ''}`}>
          {!!items?.length &&
            items.map(item => (
              <div>
                <h5>{item.header}</h5>
                <p>{item.paragraph}</p>
              </div>
            ))}
        </div>
      )}
    </div>
  );
};

export default Popover;
```