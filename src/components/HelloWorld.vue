<template>
  <div>Selam</div>
</template>

// @import url("./styles/_resettable.scss");
// @mixin theme-colors($light-theme: true) {
//   @if $light-theme {
//     --vue-data-table-bg-color: #fff;
//     --vue-data-table-text-color: #212529;
//     --vue-data-table-border-color: rgba(0, 0, 0, 0.1);
//     --vue-data-table-stripe-color: rgba(0, 0, 0, 0.05);
//     --vue-data-table-shadow-color: rgba(83, 83, 83, 0.4);
//   } @else {
//     --vue-data-table-bg-color: #212529;
//     --vue-data-table-text-color: #fff;
//     --vue-data-table-border-color: rgba(255, 255, 255, 0.15);
//     --vue-data-table-stripe-color: #32383e;
//     --vue-data-table-shadow-color: rgb(92, 92, 92);
//   }
// }

// .__vue-data-table-container {
//   &.__vue-data-table-dark {
//     @include theme-colors($light-theme: false);
//   }
//   @include theme-colors($light-theme: true);
//   -webkit-font-smoothing: antialiased;
//   -moz-osx-font-smoothing: grayscale;
//   background: var(--vue-data-table-bg-color);
//   color: var(--vue-data-table-text-color);
//   &.__vue-data-table-border {
//     table {
//       thead tr {
//         border-bottom: 1px solid var(--vue-data-table-border-color);
//       }
//       tbody tr {
//         border-bottom: 1px solid var(--vue-data-table-border-color);
//       }
//       & + .__vue-data-table-pagination {
//         border-bottom: 1px solid var(--vue-data-table-border-color);
//       }
//     }
//   }
//   &.__vue-data-table-border-outside {
//     border: 1px solid var(--vue-data-table-border-color);
//     .__vue-data-table-pagination {
//       border: none !important;
//     }
//   }
//   &.__vue-data-table-stripe {
//     table tbody tr:nth-child(odd) {
//       background-color: var(--vue-data-table-stripe-color);
//     }
//   }
//   &.__vue-data-table-hover {
//     table tbody tr:hover {
//       background-color: var(--vue-data-table-stripe-color);
//     }
//   }
//   &.__vue-data-table-large {
//     table {
//       thead tr th {
//         padding: 18px 0px;
//       }
//       tbody tr td {
//         padding: 18px 0px;
//       }
//     }
//   }
//   &.__vue-data-table-small {
//     table {
//       thead tr th {
//         padding: 8px 4px;
//       }
//       tbody tr td {
//         padding: 8px 4px;
//       }
//     }
//   }
//   &.__vue-data-table-radius {
//     border-radius: 6px;
//     tbody tr:last-child {
//       border: none !important;
//     }
//   }
//   &.__vue-data-table-radius-lg {
//     border-radius: 14px;
//     tbody tr:last-child {
//       border: none !important;
//     }
//   }
//   &.__vue-data-table-shadow {
//     box-shadow: 2px 2px 6px -1px var(--vue-data-table-shadow-color);
//   }
//   &.__vue-data-table-align-left table {
//     text-align: left;
//   }
//   &.__vue-data-table-align-right table {
//     text-align: right;
//   }
//   table {
//     border-radius: 14px;
//     thead {
//       tr {
//         font-weight: bold;
//         th {
//           padding: 13px 0px;
//         }
//       }
//     }
//     tbody tr td {
//       padding: 13px 0px;
//     }
//   }
//   .__vue-data-table-search,
//   .__vue-data-table-pagination {
//     display: flex;
//     align-items: center;
//   }
// }