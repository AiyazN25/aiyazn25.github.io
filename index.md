### Angular 15 Cheat Sheet: Essentials for Beginners

This cheat sheet covers the basic concepts you need to get started with Angular 15, focusing on how to implement loops, conditionals, and other essential features.

---

### **1. Setting Up an Angular Project**

- **Install Angular CLI:**
  ```bash
  npm install -g @angular/cli
  ```
- **Create a New Angular Project:**
  ```bash
  ng new my-angular-app
  ```
- **Run the Angular App:**
  ```bash
  cd my-angular-app
  ng serve
  ```
  Open your browser and go to `http://localhost:4200`.

---

### **2. Components**

- **Create a Component:**
  ```bash
  ng generate component component-name
  ```
- **Basic Component Structure:**

  ```typescript
  // component-name.component.ts
  import { Component } from "@angular/core";

  @Component({
    selector: "app-component-name",
    templateUrl: "./component-name.component.html",
    styleUrls: ["./component-name.component.css"],
  })
  export class ComponentNameComponent {
    title = "Hello Angular!";
  }
  ```

---

### **3. Templates (HTML)**

- **Interpolation (Display Data):**

  ```html
  <h1>{{ title }}</h1>
  ```

- **Property Binding:**

  ```html
  <input [value]="title" />
  ```

- **Event Binding:**

  ```html
  <button (click)="doSomething()">Click Me</button>
  ```

- **Two-Way Data Binding:**  
  First, import `FormsModule` in your module:

  ```typescript
  import { FormsModule } from "@angular/forms";

  @NgModule({
    imports: [FormsModule],
  })
  export class AppModule {}
  ```

  Then use `[(ngModel)]`:

  ```html
  <input [(ngModel)]="title" />
  <p>{{ title }}</p>
  ```

---

### **4. Directives**

- **NgIf (Conditionals):**

  ```html
  <p *ngIf="isVisible">This is visible</p>
  <p *ngIf="!isVisible">This is hidden</p>
  ```

- **NgFor (Loops):**

  ```html
  <ul>
    <li *ngFor="let item of items">{{ item }}</li>
  </ul>
  ```

  ```typescript
  items = ["Item 1", "Item 2", "Item 3"];
  ```

- **NgSwitch (Multiple Conditions):**

  ```html
  <div [ngSwitch]="color">
    <p *ngSwitchCase="'red'">Red</p>
    <p *ngSwitchCase="'blue'">Blue</p>
    <p *ngSwitchDefault>Other Color</p>
  </div>
  ```

- **NgClass (Conditional CSS Classes):**

  ```html
  <div [ngClass]="{ 'class-one': isClassOne, 'class-two': !isClassOne }">
    This div has conditional classes.
  </div>
  ```

  ```typescript
  isClassOne = true;
  ```

- **NgStyle (Inline Styles):**
  ```html
  <div [ngStyle]="{ 'background-color': isRed ? 'red' : 'blue' }">
    This div has conditional inline styles.
  </div>
  ```
  ```typescript
  isRed = true;
  ```

---

### **5. Input and Output**

- **@Input (Passing Data from Parent to Child Component):**

  ```typescript
  // child.component.ts
  import { Component, Input } from "@angular/core";

  @Component({
    selector: "app-child",
    template: "<p>{{ childData }}</p>",
  })
  export class ChildComponent {
    @Input() childData: string = "";
  }
  ```

  ```html
  <!-- parent.component.html -->
  <app-child [childData]="parentData"></app-child>
  ```

  ```typescript
  // parent.component.ts
  export class ParentComponent {
    parentData = "Hello from Parent!";
  }
  ```

- **@Output and EventEmitter (Sending Data from Child to Parent Component):**

  ```typescript
  // child.component.ts
  import { Component, Output, EventEmitter } from "@angular/core";

  @Component({
    selector: "app-child",
    template: `<button (click)="sendData()">Send Data</button>`,
  })
  export class ChildComponent {
    @Output() dataEmitter = new EventEmitter<string>();

    sendData() {
      this.dataEmitter.emit("Hello from Child!");
    }
  }
  ```

  ```html
  <!-- parent.component.html -->
  <app-child (dataEmitter)="receiveData($event)"></app-child>
  ```

  ```typescript
  // parent.component.ts
  export class ParentComponent {
    receiveData(data: string) {
      console.log(data); // Outputs: Hello from Child!
    }
  }
  ```

---

### **6. Services**

- **Create a Service:**

  ```bash
  ng generate service service-name
  ```

- **Injecting a Service:**

  ```typescript
  import { ServiceNameService } from './service-name.service';

  constructor(private serviceName: ServiceNameService) { }
  ```

- **Using a Service in a Component:**

  ```typescript
  export class ComponentNameComponent {
    data: any;

    constructor(private serviceName: ServiceNameService) {}

    ngOnInit() {
      this.data = this.serviceName.getData();
    }
  }
  ```

---

### **7. Routing**

- **Setting Up Routes:**

  ```typescript
  import { NgModule } from "@angular/core";
  import { RouterModule, Routes } from "@angular/router";
  import { ComponentOne } from "./component-one/component-one.component";
  import { ComponentTwo } from "./component-two/component-two.component";

  const routes: Routes = [
    { path: "component-one", component: ComponentOne },
    { path: "component-two", component: ComponentTwo },
    { path: "", redirectTo: "/component-one", pathMatch: "full" },
  ];

  @NgModule({
    imports: [RouterModule.forRoot(routes)],
    exports: [RouterModule],
  })
  export class AppRoutingModule {}
  ```

- **Linking to Routes:**

  ```html
  <a routerLink="/component-one">Go to Component One</a>
  <a routerLink="/component-two">Go to Component Two</a>
  ```

- **Displaying Routed Components:**
  ```html
  <router-outlet></router-outlet>
  ```

---

### **8. HTTP Client**

- **Importing HttpClientModule:**

  ```typescript
  import { HttpClientModule } from "@angular/common/http";

  @NgModule({
    imports: [HttpClientModule],
  })
  export class AppModule {}
  ```

- **Using HttpClient in a Service:**

  ```typescript
  import { HttpClient } from '@angular/common/http';

  constructor(private http: HttpClient) { }

  getData() {
    return this.http.get('https://api.example.com/data');
  }
  ```

---

### **9. Forms**

- **Template-Driven Form:**

  ```html
  <form #form="ngForm" (ngSubmit)="onSubmit(form)">
    <input name="name" ngModel required />
    <button type="submit">Submit</button>
  </form>
  ```

- **Reactive Form:**

  ```typescript
  import { FormBuilder, FormGroup } from '@angular/forms';

  constructor(private fb: FormBuilder) { }

  form: FormGroup = this.fb.group({
    name: ['']
  });

  onSubmit() {
    console.log(this.form.value);
  }
  ```

  ```html
  <form [formGroup]="form" (ngSubmit)="onSubmit()">
    <input formControlName="name" />
    <button type="submit">Submit</button>
  </form>
  ```

---

### **10. Pipes**

- **Built-In Pipes:**

  ```html
  <p>{{ title | uppercase }}</p>
  <!-- Converts to uppercase -->
  <p>{{ today | date }}</p>
  <!-- Formats date -->
  ```

- **Custom Pipe:**

  ```bash
  ng generate pipe pipe-name
  ```

  ```typescript
  import { Pipe, PipeTransform } from "@angular/core";

  @Pipe({
    name: "pipeName",
  })
  export class PipeNamePipe implements PipeTransform {
    transform(value: string): string {
      return "Prefix " + value;
    }
  }
  ```

  ```html
  <p>{{ title | pipeName }}</p>
  ```

---

### **11. Dependency Injection (DI)**

- **Injecting a Service:**

  ```typescript
  import { Injectable } from "@angular/core";

  @Injectable({
    providedIn: "root",
  })
  export class MyService {
    constructor() {}
  }
  ```

- **Constructor Injection:**
  ```typescript
  constructor(private myService: MyService) {}
  ```

---

### **12. Lifecycle Hooks**

- **Common Lifecycle Hooks:**

  ```typescript
  export class ComponentNameComponent implements OnInit, OnDestroy {
    ngOnInit() {
      // Called once the component is initialized
    }

    ngOnDestroy() {
      // Called once the component is destroyed
    }
  }
  ```

---

This cheat sheet provides a

quick reference for the essentials in Angular 15, helping you get up and running with your Angular development.
