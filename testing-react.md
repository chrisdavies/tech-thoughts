```javascript
import TestUtils from 'react-addons-test-utils';
import R from 'ramda';

export function componentToArray(component) {
  let result = [];

  (function crawl(item) {
    result.push(item);

    if (item.props && item.props.children) {
      if (item.props.children.forEach) {
        item.props.children.forEach(crawl);
      } else if (item.props.children.props) {
        result.push(item.props.children);
      }
    }
  }(component))

  return result;
}

export function renderToArray(component) {
  const renderer = TestUtils.createRenderer();
  renderer.render(component);
  return componentToArray(renderer.getRenderOutput());
}

export const withClass = cls => item => 
  R.contains(cls, item.props.className);

export const withText = txt => item => 
  R.contains(txt, item.props.children);
```
