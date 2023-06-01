#js/storybook 

---
2021-07-19

# チュートリアル

Atomic デザインの本やっていたのだが、古くてアゲインストになってきたので、本家のチュートリアルをやってみる。

この本ではプロジェクトと別と別で立てるようだが、本家サイトでは、プロジェクトの中に立てるっぽい。

https://storybook.js.org/

## Intro to Storybook

https://storybook.js.org/tutorials/intro-to-storybook/

### 1. Get started

```shell
$ npx degit chromaui/intro-storybook-react-template taskbox

$ cd taskbox

$ yarn  # npm じゃエラーでだめだった
```

degit って? 
https://github.com/Rich-Harris/degit

簡単プロジェクトスキャフォールドとある。

https://github.com/chromaui

GitHub の Chromatic さんとこのテンプレートをダウンロードするようだ。

確認する。
```shell
$ yarn test --watchAll
$ yarn storybook
$ yarn start
```

![[Pasted image 20210719180206.png]]

### 2. Simple component

Compoint - Driven - Development (CDD)

シンプルなコンポーネントから、storybook で複合させて、ページを作るといった手順。


redux を組み込んでもいける。

## code

### Storybook 系
#### .storybook/main.js
```js
module.exports = {
  stories: ['../src/components/**/*.stories.@(js|jsx|ts|tsx)'],
  addons: [
    '@storybook/addon-links',
    '@storybook/addon-essentials',
    '@storybook/preset-create-react-app',
  ],
}
```

#### .storybook/preview.js
このCSS は面白いので、後ろの方で採取する。
```js
import '../src/index.css'

export const parameters = {
  actions: { argTypesRegex: '^on[A-Z].*' },
  controls: {
    matchers: {
      color: /(background|color)$/i,
      date: /Date$/,
    },
  },
}
```

#### src/storybook.test.js
```js
import initStoryshots from '@storybook/addon-storyshots'
initStoryshots()
```

### Taskコンポーネント系

#### src/components/Tasks.js

```js
import React from 'react'
import PropTypes from 'prop-types'

export default function Task({
  task: { id, title, state },
  onArchiveTask,
  onPinTask,
}) {
  return (
    <div className={`list-item ${state}`}>
      <label className='checkbox'>
        <input
          type='checkbox'
          defaultChecked={state === 'TASK_ARCHIVED'}
          disabled={true}
          name='checked'
        />
        <span className='checkbox-custom' onClick={() => onArchiveTask(id)} />
      </label>
      <div className='title'>
        <input
          type='text'
          value={title}
          readOnly={true}
          placeholder='Input title'
        />
      </div>
      <div className='actions' onClick={(event) => event.stopPropagation()}>
        {state !== 'TASK_ARCHIVED' && (
          <a onClick={() => onPinTask(id)}>
            <span className={`icon-star`} />
          </a>
        )}
      </div>
    </div>
  )
}

Task.propTypes = {
  task: PropTypes.shape({
    id: PropTypes.string.isRequired,
    title: PropTypes.string.isRequired,
    state: PropTypes.string.isRequired,
  }),
  onArchiveTask: PropTypes.func,
  onPinTask: PropTypes.func,
}
```



#### src/components/Task.stories.js

```js
import React from 'react'
import Task from './Task'

export default {
  component: Task,
  title: 'Task',
}

const Template = (args) => <Task {...args} />

export const Default = Template.bind({})
Default.args = {
  task: {
    id: '1',
    title: 'Test Task',
    state: 'TASK_INBOX',
    updatedAt: new Date(2021, 0, 1, 9, 0),
  },
}

export const Pinned = Template.bind({})
Pinned.args = {
  task: {
    ...Default.args.task,
    state: 'TASK_PINNED',
  },
}

export const Archived = Template.bind({})
Archived.args = {
  task: {
    ...Default.args.task,
    state: 'TASK_ARCHIVED',
  },
}

```



### TaskList 複合コンポーネント系

#### src/components/TaskList.js
```js
import React from 'react'
import PropTypes from 'prop-types'

import Task from './Task'

import { connect } from 'react-redux'
import { archiveTask, pinTask } from '../lib/redux'

export function PureTaskList({ loading, tasks, onPinTask, onArchiveTask }) {
  const events = {
    onPinTask,
    onArchiveTask,
  }

  const LoadingRow = (
    <div className='loading-item'>
      <span className='glow-checkbox' />
      <span className='glow-text'>
        <span>Loading</span> <span>cool</span> <span>state</span>
      </span>
    </div>
  )

  if (loading) {
    return (
      <div className='list-items'>
        {LoadingRow}
        {LoadingRow}
        {LoadingRow}
        {LoadingRow}
        {LoadingRow}
        {LoadingRow}
      </div>
    )
  }
  if (tasks.length === 0) {
    return (
      <div className='list-items'>
        <div className='wrapper-message'>
          <span className='icon-check' />
          <div className='title-message'>You have no tasks</div>
          <div className='subtitle-message'>Sit back and relax</div>
        </div>
      </div>
    )
  }

  const tasksInOrder = [
    ...tasks.filter((t) => t.state === 'TASK_PINNED'),
    ...tasks.filter((t) => t.state !== 'TASK_PINNED'),
  ]
  return (
    <div className='list-items'>
      {tasksInOrder.map((task) => (
        <Task key={task.id} task={task} {...events} />
      ))}
    </div>
  )
}

PureTaskList.propTypes = {
  loading: PropTypes.bool,
  tasks: PropTypes.arrayOf(Task.propTypes.task).isRequired,
  onPinTask: PropTypes.func.isRequired,
  onArchiveTask: PropTypes.func.isRequired,
}

PureTaskList.defaultProps = {
  loading: false,
}

export default connect(
  ({ tasks }) => ({
    tasks: tasks.filter(
      (t) => t.state === 'TASK_INBOX' || t.state === 'TASK_PINNED',
    ),
  }),
  (dispatch) => ({
    onArchiveTask: (id) => dispatch(archiveTask(id)),
    onPinTask: (id) => dispatch(pinTask(id)),
  }),
)(PureTaskList)

```





#### src/components/TaskList.stories.js

```js
import React from 'react'
import { PureTaskList } from './TaskList'
import * as TaskStories from './Task.stories'

export default {
  component: PureTaskList,
  title: 'TaskList',
  decorators: [(story) => <div style={{ padding: '3rem' }}>{story()}</div>],
}

const Template = (args) => <PureTaskList {...args} />

export const Default = Template.bind({})
Default.args = {
  tasks: [
    { ...TaskStories.Default.args.task, id: '1', title: 'Task 1' },
    { ...TaskStories.Default.args.task, id: '2', title: 'Task 2' },
    { ...TaskStories.Default.args.task, id: '3', title: 'Task 3' },
    { ...TaskStories.Default.args.task, id: '4', title: 'Task 4' },
    { ...TaskStories.Default.args.task, id: '5', title: 'Task 5' },
    { ...TaskStories.Default.args.task, id: '6', title: 'Task 6' },
  ],
}

export const WithPinnedTasks = Template.bind({})
WithPinnedTasks.args = {
  tasks: [
    ...Default.args.tasks.slice(0, 5),
    { id: '6', title: 'Task 6 (pinned)', state: 'TASK_PINNED' },
  ],
}

export const Loading = Template.bind({})
Loading.args = {
  tasks: [],
  loading: true,
}

export const Empty = Template.bind({})
Empty.args = {
  ...Loading.args,
  loading: false,
}

```

#### src/components/TaskList.test.js

```js
import { render } from '@testing-library/react'
import { composeStories } from '@storybook/testing-react'
import * as TaskListStories from './TaskList.stories'

const { WithPinnedTasks } = composeStories(TaskListStories)

it('renders pinned tasks at the start of the list', () => {
  const { container } = render(<WithPinnedTasks />)

  expect(
    container.querySelector(
      '.list-item:nth-child(1) input[value="Task 6 (pinned)"]',
    ),
  ).not.toBe(null)
})

```

### InBoxScreen ページコンポーネント系

#### src/components/InboxScreen.js

```js
import React from 'react'
import PropTypes from 'prop-types'

import { connect } from 'react-redux'
import TaskList from './TaskList'

export function PureInboxScreen({ error }) {
  if (error) {
    return (
      <div className='page lists-show'>
        <div className='wrapper-message'>
          <span className='icon-face-sad' />
          <div className='title-message'>Oh no!</div>
          <div className='subtitle-message'>Something went wrong</div>
        </div>
      </div>
    )
  }
  return (
    <div className='page lists-show'>
      <nav>
        <h1 className='title-page'>
          <span className='title-wrapper'>Taskbox</span>
        </h1>
      </nav>
      <TaskList />
    </div>
  )
}

PureInboxScreen.propTypes = {
  error: PropTypes.string,
}

PureInboxScreen.defaultProps = {
  error: null,
}

export default connect(({ error }) => ({ error }))(PureInboxScreen)

```

#### src/components/InboxScreen.stories.js

```js
import React from 'react'
import { Provider } from 'react-redux'
import { PureInboxScreen } from './InboxScreen'

import { action } from '@storybook/addon-actions'
import * as TaskListStories from './TaskList.stories'

// A super-simple mock of redux store
const store = {
  getState: () => {
    return {
      tasks: TaskListStories.Default.args.tasks,
    }
  },
  subscribe: () => 0,
  dispatch: action('dispatch'),
}

export default {
  component: PureInboxScreen,
  decorators: [(story) => <Provider store={store}>{story()}</Provider>],
  title: 'InboxScreen',
}

const Template = (args) => <PureInboxScreen {...args} />

export const Default = Template.bind({})

export const Error = Template.bind({})
Error.args = {
  error: 'Something',
}

```



### Redux 系

#### src/lib/redux.js

```js
import { createStore } from 'redux'

export const actions = {
  ARCHIVE_TASK: 'ARCHIVE_TASKE',
  PIN_TASK: 'PIN_TASK',
}

export const archiveTask = (id) => ({ type: actions.ARCHIVE_TASK, id })
export const pinTask = (id) => ({ type: actions.PIN_TASK, id })

function taskStateReducer(taskState) {
  return (state, action) => {
    return {
      ...state,
      tasks: state.tasks.map((task) =>
        task.id === action.id ? { ...task, state: taskState } : task,
      ),
    }
  }
}

export const reducer = (state, action) => {
  switch (action.type) {
    case actions.ARCHIVE_TASK:
      return taskStateReducer('TASK_ARCHIVED')(state, action)
    case action.PIN_TASK:
      return taskStateReducer('TASK_PINNED')(state, action)
    default:
      return state
  }
}

const defaultTasks = [
  { id: '1', title: 'Something', state: 'TASK_INBOX' },
  { id: '2', title: 'Something more', state: 'TASK_INBOX' },
  { id: '3', title: 'Something else', state: 'TASK_INBOX' },
  { id: '4', title: 'Something again', state: 'TASK_INBOX' },
]

export default createStore(reducer, { tasks: defaultTasks })
```



### CSS

これはもらっとけ。

#### src/index.css

```css
/* Reset.less
 * Props to Eric Meyer (meyerweb.com) for his CSS reset file. We're using an adapted version here	that cuts out some of the reset HTML elements we will never need here (i.e., dfn, samp, etc).
 * ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- */
 html,
 body {
   margin: 0;
   padding: 0;
 }
 h1,
 h2,
 h3,
 h4,
 h5,
 h6,
 p,
 blockquote,
 pre,
 a,
 abbr,
 acronym,
 address,
 cite,
 code,
 del,
 dfn,
 em,
 img,
 q,
 s,
 samp,
 small,
 strike,
 strong,
 sub,
 sup,
 tt,
 var,
 dd,
 dl,
 dt,
 li,
 ol,
 ul,
 fieldset,
 form,
 label,
 legend,
 button,
 table,
 caption,
 tbody,
 tfoot,
 thead,
 tr,
 th,
 td {
   margin: 0;
   padding: 0;
   border: 0;
   font-weight: normal;
   font-style: normal;
   font-size: 100%;
   line-height: 1;
   font-family: inherit;
 }
 table {
   border-collapse: collapse;
   border-spacing: 0;
 }
 ol,
 ul {
   list-style: none;
 }
 q:before,
 q:after,
 blockquote:before,
 blockquote:after {
   content: "";
 }
 html {
   font-size: 100%;
   -webkit-text-size-adjust: 100%;
   -ms-text-size-adjust: 100%;
 }
 a:focus {
   outline: thin dotted;
 }
 a:hover,
 a:active {
   outline: 0;
 }
 article,
 aside,
 details,
 figcaption,
 figure,
 footer,
 header,
 hgroup,
 nav,
 section {
   display: block;
 }
 audio,
 canvas,
 video {
   display: inline-block;
   *display: inline;
   *zoom: 1;
 }
 audio:not([controls]) {
   display: none;
 }
 sub,
 sup {
   font-size: 75%;
   line-height: 0;
   position: relative;
   vertical-align: baseline;
 }
 sup {
   top: -0.5em;
 }
 sub {
   bottom: -0.25em;
 }
 img {
   border: 0;
   -ms-interpolation-mode: bicubic;
 }
 button,
 input,
 select,
 textarea {
   font-size: 100%;
   margin: 0;
   vertical-align: baseline;
   *vertical-align: middle;
 }
 button,
 input {
   line-height: normal;
   *overflow: visible;
 }
 button::-moz-focus-inner,
 input::-moz-focus-inner {
   border: 0;
   padding: 0;
 }
 button,
 input[type="button"],
 input[type="reset"],
 input[type="submit"] {
   cursor: pointer;
   -webkit-appearance: button;
 }
 input[type="search"] {
   -webkit-appearance: textfield;
   -webkit-box-sizing: content-box;
   -moz-box-sizing: content-box;
   box-sizing: content-box;
 }
 input[type="search"]::-webkit-search-decoration {
   -webkit-appearance: none;
 }
 textarea {
   overflow: auto;
   vertical-align: top;
 }
 @keyframes spin {
   0% {
     transform: rotate(0deg);
   }
   100% {
     transform: rotate(359deg);
   }
 }
 @keyframes glow {
   0%,
   100% {
     opacity: 1;
   }
   50% {
     opacity: 0.5;
   }
 }
 @font-face {
   font-family: 'Nunito Sans';
   font-style: italic;
   font-weight: 400;
   src: url(https://fonts.gstatic.com/s/nunitosans/v6/pe0oMImSLYBIv1o4X1M8cce4E9lKcw.ttf) format('truetype');
 }
 @font-face {
   font-family: 'Nunito Sans';
   font-style: normal;
   font-weight: 400;
   src: url(https://fonts.gstatic.com/s/nunitosans/v6/pe0qMImSLYBIv1o4X1M8cce9I94.ttf) format('truetype');
 }
 @font-face {
   font-family: 'Nunito Sans';
   font-style: normal;
   font-weight: 800;
   src: url(https://fonts.gstatic.com/s/nunitosans/v6/pe03MImSLYBIv1o4X1M8cc8aBc5tU1Q.ttf) format('truetype');
 }
 .force-wrap {
   word-wrap: break-word;
   word-break: break-all;
   -ms-word-break: break-all;
   word-break: break-word;
   hyphens: auto;
 }
 .type-light {
   font-family: 'Nunito Sans', "Helvetica Neue", Helvetica, Arial, sans-serif;
   font-weight: 300;
 }
 .type-bold {
   font-family: 'Nunito Sans', "Helvetica Neue", Helvetica, Arial, sans-serif;
   font-weight: 800;
 }
 .type-italic {
   font-family: 'Nunito Sans', "Helvetica Neue", Helvetica, Arial, sans-serif;
   font-weight: 400;
   font-style: italic;
 }
 * {
   box-sizing: border-box;
   -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
   -webkit-tap-highlight-color: transparent;
 }
 html,
 button,
 input,
 textarea,
 select {
   outline: none;
   -webkit-font-smoothing: antialiased;
   -moz-osx-font-smoothing: grayscale;
 }
 body {
   font-family: 'Nunito Sans', "Helvetica Neue", Helvetica, Arial, sans-serif;
   font-style: 400;
   color: #333;
   font-size: 16px;
   background-color: [[26c6da]];
 }
 h1,
 h2,
 h3,
 h4,
 h5,
 h6 {
   font-family: 'Nunito Sans', "Helvetica Neue", Helvetica, Arial, sans-serif;
   font-style: 400;
   margin: 0;
   padding: 0;
 }
 h1 {
   font-size: 40px;
   line-height: 48px;
 }
 h2 {
   font-size: 28px;
   line-height: 32px;
 }
 h3 {
   font-size: 24px;
   line-height: 28px;
 }
 h4 {
   font-size: 20px;
   line-height: 24px;
 }
 h5 {
   font-size: 14px;
   line-height: 20px;
   color: #c;
   text-transform: uppercase;
 }
 h6 {
   color: #a;
 }
 p {
   font-size: 16px;
   line-height: 24px;
 }
 sub,
 sup {
   font-size: 0.8em;
 }
 sub {
   bottom: -0.2em;
 }
 sup {
   top: -0.2em;
 }
 b {
   font-weight: bold;
 }
 em {
   font-style: italic;
 }
 input[type="text"],
 input[type="email"],
 input[type="password"],
 textarea {
   font-size: 14px;
   line-height: 20px;
   font-family: 'Nunito Sans', "Helvetica Neue", Helvetica, Arial, sans-serif;
   font-style: 400;
   padding: 0.75rem 0;
   line-height: 1.5rem !important;
   border: none;
   border-radius: 0;
   box-sizing: border-box;
   color: #333;
   outline: none;
 }
 input[type="text"] ::placeholder,
 input[type="email"] ::placeholder,
 input[type="password"] ::placeholder,
 textarea ::placeholder {
   color: [[778b91]];
 }
 input[type="text"][disabled],
 input[type="email"][disabled],
 input[type="password"][disabled],
 textarea[disabled] {
   opacity: 0.5;
 }
 input:-webkit-autofill {
   -webkit-box-shadow: 0 0 0 1000px white inset;
 }
 .checkbox {
   display: inline-block;
   height: 3rem;
   position: relative;
   vertical-align: middle;
   width: 44px;
 }
 .checkbox input[type="checkbox"] {
   font-size: 1em;
   visibility: hidden;
 }
 .checkbox input[type="checkbox"] + span:before {
   position: absolute;
   top: 50%;
   right: auto;
   bottom: auto;
   left: 50%;
   width: 0.85em;
   height: 0.85em;
   transform: translate3d(-50%, -50%, 0);
   background: transparent;
   box-shadow: [[2cc5d2]] 0 0 0 1px inset;
   content: '';
   display: block;
 }
 .checkbox input[type="checkbox"]:checked + span:before {
   font-size: 16px;
   line-height: 24px;
   box-shadow: none;
   color: [[2cc5d2]];
   margin-top: -1px;
   font-family: 'percolate';
   speak: none;
   font-style: normal;
   font-weight: normal;
   font-variant: normal;
   text-transform: none;
   line-height: 1;
   -webkit-font-smoothing: antialiased;
   -moz-osx-font-smoothing: grayscale;
   content: "\e65e";
 }
 .input-symbol {
   display: inline-block;
   position: relative;
 }
 .input-symbol.error [class^="icon-"],
 .input-symbol.error [class*=" icon-"] {
   color: [[ff4400]];
 }
 .input-symbol [class^="icon-"],
 .input-symbol [class*=" icon-"] {
   left: 1em;
 }
 .input-symbol input {
   padding-left: 3em;
 }
 .input-symbol input {
   width: 100%;
 }
 .input-symbol input:focus + [class^="icon-"],
 .input-symbol input:focus + [class*=" icon-"] {
   color: [[2cc5d2]];
 }
 .input-symbol [class^="icon-"],
 .input-symbol [class*=" icon-"] {
   transition: all 300ms ease-in;
   transform: translate3d(0, -50%, 0);
   background: transparent;
   color: #a;
   font-size: 1em;
   height: 1em;
   position: absolute;
   top: 50%;
   width: 1em;
 }
 @font-face {
   font-family: "percolate";
   src: url("./assets/icon/percolate.eot?-5w3um4");
   src: url("./assets/icon/percolate.eot?[[iefix5w3um4]]") format("embedded-opentype"),
     url("./assets/icon/percolate.woff?5w3um4") format("woff"),
     url("./assets/icon/percolate.ttf?5w3um4") format("truetype"),
     url("./assets/icon/percolate.svg?5w3um4") format("svg");
   font-weight: normal;
   font-style: normal;
 }
 [class^="icon-"],
 [class*=" icon-"] {
   font-family: "percolate";
   speak: none;
   font-style: normal;
   font-weight: normal;
   font-variant: normal;
   text-transform: none;
   line-height: 1;
   /* Better Font Rendering =========== */
   -webkit-font-smoothing: antialiased;
   -moz-osx-font-smoothing: grayscale;
 }
 .icon-graphql:before {
   content: "\e90a";
 }
 .icon-redux:before {
   content: "\e908";
 }
 .icon-grid:before {
   content: "\e909";
 }
 .icon-redirect:before {
   content: "\e907";
 }
 .icon-grow:before {
   content: "\e903";
 }
 .icon-lightning:before {
   content: "\e904";
 }
 .icon-request-change:before {
   content: "\e905";
 }
 .icon-transfer:before {
   content: "\e906";
 }
 .icon-calendar:before {
   content: "\e902";
 }
 .icon-sidebar:before {
   content: "\e900";
 }
 .icon-tablet:before {
   content: "\e901";
 }
 .icon-atmosphere:before {
   content: "\e671";
 }
 .icon-browser:before {
   content: "\e672";
 }
 .icon-database:before {
   content: "\e673";
 }
 .icon-expand-alt:before {
   content: "\e674";
 }
 .icon-mobile:before {
   content: "\e675";
 }
 .icon-watch:before {
   content: "\e676";
 }
 .icon-home:before {
   content: "\e600";
 }
 .icon-user-alt:before {
   content: "\e601";
 }
 .icon-user:before {
   content: "\e602";
 }
 .icon-user-add:before {
   content: "\e603";
 }
 .icon-users:before {
   content: "\e604";
 }
 .icon-profile:before {
   content: "\e605";
 }
 .icon-bookmark:before {
   content: "\e606";
 }
 .icon-bookmark-hollow:before {
   content: "\e607";
 }
 .icon-star:before {
   content: "\e608";
 }
 .icon-star-hollow:before {
   content: "\e609";
 }
 .icon-circle:before {
   content: "\e60a";
 }
 .icon-circle-hollow:before {
   content: "\e60b";
 }
 .icon-heart:before {
   content: "\e60c";
 }
 .icon-heart-hollow:before {
   content: "\e60d";
 }
 .icon-face-happy:before {
   content: "\e60e";
 }
 .icon-face-sad:before {
   content: "\e60f";
 }
 .icon-face-neutral:before {
   content: "\e610";
 }
 .icon-lock:before {
   content: "\e611";
 }
 .icon-unlock:before {
   content: "\e612";
 }
 .icon-key:before {
   content: "\e613";
 }
 .icon-arrow-left-alt:before {
   content: "\e614";
 }
 .icon-arrow-right-alt:before {
   content: "\e615";
 }
 .icon-sync:before {
   content: "\e616";
 }
 .icon-reply:before {
   content: "\e617";
 }
 .icon-expand:before {
   content: "\e618";
 }
 .icon-arrow-left:before {
   content: "\e619";
 }
 .icon-arrow-up:before {
   content: "\e61a";
 }
 .icon-arrow-down:before {
   content: "\e61b";
 }
 .icon-arrow-right:before {
   content: "\e61c";
 }
 .icon-chevron-down:before {
   content: "\e61d";
 }
 .icon-back:before {
   content: "\e61e";
 }
 .icon-download:before {
   content: "\e61f";
 }
 .icon-upload:before {
   content: "\e620";
 }
 .icon-proceed:before {
   content: "\e621";
 }
 .icon-info:before {
   content: "\e622";
 }
 .icon-question:before {
   content: "\e623";
 }
 .icon-alert:before {
   content: "\e624";
 }
 .icon-edit:before {
   content: "\e625";
 }
 .icon-paintbrush:before {
   content: "\e626";
 }
 .icon-close:before {
   content: "\e627";
 }
 .icon-trash:before {
   content: "\e628";
 }
 .icon-cross:before {
   content: "\e629";
 }
 .icon-delete:before {
   content: "\e62a";
 }
 .icon-power:before {
   content: "\e62b";
 }
 .icon-add:before {
   content: "\e62c";
 }
 .icon-plus:before {
   content: "\e62d";
 }
 .icon-document:before {
   content: "\e62e";
 }
 .icon-graph-line:before {
   content: "\e62f";
 }
 .icon-doc-chart:before {
   content: "\e630";
 }
 .icon-doc-list:before {
   content: "\e631";
 }
 .icon-category:before {
   content: "\e632";
 }
 .icon-copy:before {
   content: "\e633";
 }
 .icon-book:before {
   content: "\e634";
 }
 .icon-certificate:before {
   content: "\e636";
 }
 .icon-print:before {
   content: "\e637";
 }
 .icon-list-unordered:before {
   content: "\e638";
 }
 .icon-graph-bar:before {
   content: "\e639";
 }
 .icon-menu:before {
   content: "\e63a";
 }
 .icon-filter:before {
   content: "\e63b";
 }
 .icon-ellipsis:before {
   content: "\e63c";
 }
 .icon-cog:before {
   content: "\e63d";
 }
 .icon-wrench:before {
   content: "\e63e";
 }
 .icon-nut:before {
   content: "\e63f";
 }
 .icon-camera:before {
   content: "\e640";
 }
 .icon-eye:before {
   content: "\e641";
 }
 .icon-photo:before {
   content: "\e642";
 }
 .icon-video:before {
   content: "\e643";
 }
 .icon-speaker:before {
   content: "\e644";
 }
 .icon-phone:before {
   content: "\e645";
 }
 .icon-flag:before {
   content: "\e646";
 }
 .icon-pin:before {
   content: "\e647";
 }
 .icon-compass:before {
   content: "\e648";
 }
 .icon-globe:before {
   content: "\e649";
 }
 .icon-location:before {
   content: "\e64a";
 }
 .icon-search:before {
   content: "\e64b";
 }
 .icon-timer:before {
   content: "\e64c";
 }
 .icon-time:before {
   content: "\e64d";
 }
 .icon-dashboard:before {
   content: "\e64e";
 }
 .icon-hourglass:before {
   content: "\e64f";
 }
 .icon-play:before {
   content: "\e650";
 }
 .icon-stop:before {
   content: "\e651";
 }
 .icon-email:before {
   content: "\e652";
 }
 .icon-comment:before {
   content: "\e653";
 }
 .icon-link:before {
   content: "\e654";
 }
 .icon-paperclip:before {
   content: "\e655";
 }
 .icon-box:before {
   content: "\e656";
 }
 .icon-structure:before {
   content: "\e657";
 }
 .icon-commit:before {
   content: "\e658";
 }
 .icon-cpu:before {
   content: "\e659";
 }
 .icon-memory:before {
   content: "\e65a";
 }
 .icon-outbox:before {
   content: "\e65b";
 }
 .icon-share:before {
   content: "\e65c";
 }
 .icon-button:before {
   content: "\e65d";
 }
 .icon-check:before {
   content: "\e65e";
 }
 .icon-form:before {
   content: "\e65f";
 }
 .icon-admin:before {
   content: "\e660";
 }
 .icon-paragraph:before {
   content: "\e661";
 }
 .icon-bell:before {
   content: "\e662";
 }
 .icon-rss:before {
   content: "\e663";
 }
 .icon-basket:before {
   content: "\e664";
 }
 .icon-credit:before {
   content: "\e665";
 }
 .icon-support:before {
   content: "\e666";
 }
 .icon-shield:before {
   content: "\e667";
 }
 .icon-beaker:before {
   content: "\e668";
 }
 .icon-google:before {
   content: "\e669";
 }
 .icon-gdrive:before {
   content: "\e66a";
 }
 .icon-youtube:before {
   content: "\e66b";
 }
 .icon-facebook:before {
   content: "\e66c";
 }
 .icon-thumbs-up:before {
   content: "\e66d";
 }
 .icon-twitter:before {
   content: "\e66e";
 }
 .icon-github:before {
   content: "\e66f";
 }
 .icon-meteor:before {
   content: "\e670";
 }
 a {
   transition: all 200ms ease-in;
   color: [[5db9ff]];
   cursor: pointer;
   text-decoration: none;
 }
 a:hover {
   color: [[239da8]];
 }
 a:active {
   color: #555;
 }
 a:focus {
   outline: none;
 }
 .list-heading {
   letter-spacing: 0.3em;
   text-indent: 0.3em;
   text-transform: uppercase;
   font-family: 'Nunito Sans', "Helvetica Neue", Helvetica, Arial, sans-serif;
   font-weight: 800;
   font-size: 11px;
   padding-left: 15px;
   line-height: 40px;
   background: [[f8f8f8]];
   color: #a;
 }
 .list-heading .icon-sync {
   opacity: 1;
   animation: spin 2s infinite linear;
   display: inline-block;
   margin-right: 4px;
 }
 .list-item {
   font-size: 14px;
   line-height: 20px;
   display: flex;
   flex-wrap: wrap;
   height: 3rem;
   width: 100%;
   background: white;
   transition: all ease-out 150ms;
 }
 .list-item .title {
   overflow: hidden;
   text-overflow: ellipsis;
   white-space: nowrap;
   flex: 1;
 }
 .list-item input[type="text"] {
   background: transparent;
   width: 100%;
 }
 .list-item input[type="text"]:focus {
   cursor: text;
 }
 .list-item .actions {
   transition: all 200ms ease-in;
   padding-right: 20px;
 }
 .list-item .actions a {
   display: inline-block;
   vertical-align: top;
   text-align: center;
   color: #e;
 }
 .list-item .actions a:hover {
   color: [[2cc5d2]];
 }
 .list-item .actions a:active {
   color: #555;
 }
 .list-item .actions [class^="icon-"] {
   font-size: 16px;
   line-height: 24px;
   line-height: 3rem;
   text-align: center;
 }
 .list-item.TASK_PINNED .icon-star {
   color: [[2cc5d2]];
 }
 .list-item.TASK_ARCHIVED input[type="text"] {
   color: #a;
 }
 .list-item:hover {
   background-image: linear-gradient(to bottom, [[e5f9f7]] 0%, [[f0fffd]] 100%);
 }
 .list-item:hover .checkbox {
   cursor: pointer;
 }
 .list-item + .list-item {
   border-top: 1px solid [[f0f9fb]];
 }
 .list-item.checked input[type="text"] {
   color: #c;
   text-decoration: line-through;
 }
 .list-item.checked .delete-item {
   display: inline-block;
 }
 .loading-item {
   height: 3rem;
   width: 100%;
   background: white;
   display: flex;
   align-items: center;
   line-height: 1rem;
   padding-left: 16px;
 }
 .loading-item .glow-checkbox,
 .loading-item .glow-text span {
   animation: glow 1.5s ease-in-out infinite;
   background: #e;
   color: transparent;
   cursor: progress;
   display: inline-block;
 }
 .loading-item .glow-checkbox {
   margin-right: 16px;
   width: 12px;
   height: 12px;
 }
 .loading-item + .loading-item {
   border-top: 1px solid [[f0f9fb]];
 }
 .list-items {
   position: relative;
   background: white;
   min-height: 288px;
 }
 .list-items .select-placeholder {
   border: none;
   width: 48px;
 }
 .wrapper-message {
   position: absolute;
   top: 45%;
   right: 0;
   bottom: auto;
   left: 0;
   width: auto;
   height: auto;
   transform: translate3d(0, -50%, 0);
   text-align: center;
 }
 .wrapper-message [class^="icon-"],
 .wrapper-message [class*=" icon-"] {
   font-size: 48px;
   line-height: 56px;
   color: [[2cc5d2]];
   display: block;
 }
 .wrapper-message .title-message {
   font-size: 16px;
   line-height: 24px;
   font-family: 'Nunito Sans', "Helvetica Neue", Helvetica, Arial, sans-serif;
   font-weight: 800;
   color: #555;
 }
 .wrapper-message .subtitle-message {
   font-size: 14px;
   line-height: 20px;
   color: #666;
 }
 .page.lists-show {
   min-height: 100vh;
   background: white;
 }
 .page.lists-show nav {
   background: [[d3edf4]];
   padding: 1.5rem 1.25rem;
   text-align: center;
 }
 @media screen and (min-width: 40em) {
   .page.lists-show nav {
     text-align: left;
   }
 }
 .page.lists-show nav .title-page {
   font-size: 20px;
   line-height: 24px;
   line-height: 2rem;
   cursor: pointer;
   white-space: nowrap;
 }
 .page.lists-show nav .title-page .title-wrapper {
   overflow: hidden;
   text-overflow: ellipsis;
   white-space: nowrap;
   font-family: 'Nunito Sans', "Helvetica Neue", Helvetica, Arial, sans-serif;
   font-weight: 800;
   color: [[1c3f53]];
   display: inline-block;
   vertical-align: top;
   max-width: 100%;
 }
```

