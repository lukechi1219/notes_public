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

---

service to move data between components and keeping state of buying.

@Injectable({
providedIn: 'root',
})
export class CartService {
items: Product[] = [];
}

---

<app-product-alerts [product]="product" (notify)="onNotify()">

onNotify() {
window.alert('You will be notified when the product goes on sale');
}

ngOnInit(): void {
// First get the product id from the current route.
const routeParams = this.route.snapshot.paramMap;
const productIdFromRoute = Number(routeParams.get('productId'));

    // Find the product that correspond with the id provided in route.
    this.product = products.find(
      (product) => product.id === productIdFromRoute
    );
}

shippingCosts!: Observable<{ type: string; price: number }[]>;

<div class="shipping-item" *ngFor="let shipping of shippingCosts | async">
</div>

---

https://angular.io/start/start-deployment

Learning more Angular
For a more in-depth tutorial that leads you through building an application locally and exploring many of Angular's most popular features, see Tour of Heroes.

https://angular.io/tutorial

To explore Angular's foundational concepts, see the guides in the Understanding Angular section such as Angular Components Overview or Template syntax.

https://angular.io/guide/component-overview

https://angular.io/guide/template-syntax


---

Exploring the Angular ecosystem
To support your UX/UI development, see Angular Material.

The Angular community also has an extensive network of third-party tools and libraries.


---


https://angular.io/guide/setup-local


https://angular.io/guide/what-is-angular





















.
















.
