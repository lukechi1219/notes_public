# angular.io

# modern web platform

- web, 
- mobile web, 
- native mobile and 
- native desktop

# SPEED & PERFORMANCE

- via Web Workers and 
- server-side rendering.
- scalability
- Meet huge data requirements by building data models on 
  - RxJS, 
  - Immutable.js or 
  - another push-model
- .

# INCREDIBLE TOOLING

- focus on building amazing apps rather than trying to make the code work.

---

= https://angular.io/start =

walking you through building an e-commerce site with a catalog, shopping cart, and check-out form.

# Prerequisites
- To get the most out of this tutorial you should already have a basic understanding of the following.

.

- HTML https://developer.mozilla.org/docs/Learn/HTML
- JavaScript https://developer.mozilla.org/docs/Web/JavaScript
- TypeScript https://www.typescriptlang.org/

Components define areas of responsibility in the UI that let you reuse sets of UI functionality.

export class ProductAlertsComponent {

@Input() product!: Product;

}


<button type="button" (click)="notify.emit()">Notify Me</button>


<app-product-alerts [product]="product" (notify)="onNotify()">

onNotify() {
window.alert('You will be notified when the product goes on sale');
}




















.
















.
