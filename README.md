# vue-h-sticky
[![npm version](https://badge.fury.io/js/vue-h-sticky.svg)](https://badge.fury.io/js/vue-h-sticky)
[![npm](https://img.shields.io/npm/l/express.svg)](http://opensource.org/licenses/MIT)
[![Build Status](https://travis-ci.org/zont/vue-h-sticky.svg?branch=master)](https://travis-ci.org/zont/vue-h-sticky)

> Edge 12+, FF 36+, Chrome 49+, or use translation from ES2105 to ES5

## Introduction

`Under construction`

## Setup
```bash
npm install vue-h-sticky
```

## Example
```javascript
import stickyHeader from 'vue-h-sticky';

export default {
  template: `
    <div>
      <sticky-header>
        <table slot="cross">
          <thead>
            <tr>
              <th></th>
            </tr>
          </thead>
        </table>
        <table slot="top">
          <thead>
            <tr>
              <th></th>
            </tr>
          </thead>
        </table>
        <table slot="left">
          <tbody>
            <tr>
              <td></td>
            </tr>
          </tbody>
        </table>
        <table slot="body">
          <tbody>
            <tr>
              <td></td>
            </tr>
          </tbody>
        </table>
      </sticky-header>
    </div>
  `,
  
  components: {
    stickyHeader
  }
};
```