# notes

npx create-nx-workspace@latest

// npm install -D @nrwl/angular

npm install --save-dev @nrwl/angular

npx nx g app demo -d

ReactiveFormsModule

<form [formGroup]="checkoutForm" (ngSubmit)="onSubmit()">
  <div>
    <label for="name"> Name </label>
    <input id="name" type="text" formControlName="name" />
  </div>
  <div>
    <label for="address"> Address </label>
    <input id="address" type="text" formControlName="address" />
  </div>
  <button class="button" type="submit">Purchase</button>
</form>


