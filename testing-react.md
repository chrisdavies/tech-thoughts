Recently, I've been working with React, but I've found testing components to be a bit tricky. One of the things I did was create a set of helper functions that makes working with shallow-rendered React components much easier. Here's the helper file:

```javascript
import TestUtils from 'react-addons-test-utils';
import R from 'ramda';

const children = R.path(['props', 'children'])

export function componentToArray(component) {
  let result = [];

  (function flatten(item) {
    if (!item) {
      return;
    }

    result.push(item);
    const content = children(item);

    if (content) {
      if (content.forEach) {
        content.forEach(flatten);
      } else if (content.props) {
        result.push(content);
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

const defaultToBlank = R.defaultTo('');

const className = R.path(['props', 'className'])

export const withClass = cls =>
  R.compose(R.contains(cls), defaultToBlank, className);

export const itemText = R.compose(
  R.when(R.isArrayLike, R.join('')),
  defaultToBlank,
  children
)

export const withText = txt => item =>
  R.contains(txt, itemText(item));

```

Its functions greatly simplify the process of testing a React component. Here's a sample Jasmine test script that actually comes from our production codebase.

```javascript
import React from 'react';
import R from 'ramda';
import UploadStatus from '../../assets/javascripts/components/upload-status';
import {renderToArray, withText, withClass} from '../test-helpers/react';

describe('upload-status', function () {
  beforeEach(function() {
    this.abortCount = 0;

    const abort = () => ++this.abortCount;

    const props = {
      file: { name: 'test.txt' },
      progress: 45,
      abort: abort
    };

    this.html = renderToArray(<UploadStatus upload={props} />);
  });

  it('Displays the file name', function () {
    const fileName = R.find(withText('test.txt'));
    expect(fileName(this.html)).toBeTruthy();
  });

  it('Displays the progress bar', function () {
    const progressBar = R.find(R.pathEq(['props', 'style', 'width'], '45%'));
    expect(progressBar(this.html)).toBeTruthy();
  })

  it('Aborts the upload when canceled', function () {
    const cancelLink = R.find(withClass('upload-status-cancel-action'));

    cancelLink(this.html).props.onClick();

    expect(this.abortCount).toBe(1);
  })
});

```

You can see that finding any child of your React component is quite simple:

```javascript
// A function that finds the cancel link, invoked like this: cancelLink(component)
const cancelLink = R.find(withClass('upload-status-cancel-action'));

// A function that finds the first element that contains the text 'test.txt' (e.g. <div>File is test.txt</div>
// Called like this: fileName(component)
const fileName = R.find(withText('test.txt'));

// A function that finds the first element that looks something like <div style="width:45%">..</div>
// Called like this: progressBar(component)
const progressBar = R.find(R.pathEq(['props', 'style', 'width'], '45%'));
```

Happy testing!
