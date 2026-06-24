# 🅰️ Angular Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · Angular (TypeScript) quick reference.

---

## Setup & CLI

```bash
npm install -g @angular/cli
ng new my-app
ng serve                       # dev server :4200
ng generate component user     # or: ng g c user
ng g service data
ng g module admin
ng build --configuration production
```

## Component

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-user',
  standalone: true,
  template: `<h1>Hello {{ name }}</h1>`,
  styleUrls: ['./user.component.css'],
})
export class UserComponent {
  name = 'Alan';
  count = 0;
  increment() { this.count++; }
}
```

## Template Binding

```html
<!-- Interpolation -->
<p>{{ name }}</p>

<!-- Property binding -->
<img [src]="imageUrl" [alt]="title">

<!-- Event binding -->
<button (click)="increment()">+</button>

<!-- Two-way binding (needs FormsModule) -->
<input [(ngModel)]="name">

<!-- Class & style binding -->
<div [class.active]="isActive" [style.color]="color"></div>
```

## Directives

```html
<!-- Conditional -->
<p *ngIf="isLoggedIn; else guest">Welcome</p>
<ng-template #guest>Please log in</ng-template>

<!-- Loop -->
<li *ngFor="let item of items; let i = index; trackBy: trackById">
  {{ i }}: {{ item.name }}
</li>

<!-- Switch -->
<div [ngSwitch]="role">
  <p *ngSwitchCase="'admin'">Admin</p>
  <p *ngSwitchDefault>User</p>
</div>
```

## Input / Output (parent-child)

```typescript
import { Input, Output, EventEmitter } from '@angular/core';

export class ChildComponent {
  @Input() title!: string;            // receive from parent
  @Output() saved = new EventEmitter<string>();  // emit to parent

  save() { this.saved.emit('done'); }
}
```

```html
<app-child [title]="pageTitle" (saved)="onSaved($event)"></app-child>
```

## Services & Dependency Injection

```typescript
import { Injectable } from '@angular/core';

@Injectable({ providedIn: 'root' })
export class DataService {
  getUsers() { return ['Alan', 'Bob']; }
}

// inject in component
export class UserComponent {
  constructor(private data: DataService) {}
  users = this.data.getUsers();
}
```

## HTTP Client

```typescript
import { HttpClient } from '@angular/common/http';

@Injectable({ providedIn: 'root' })
export class ApiService {
  constructor(private http: HttpClient) {}

  getPosts() { return this.http.get<Post[]>('/api/posts'); }
  create(p: Post) { return this.http.post('/api/posts', p); }
}

// in component
this.api.getPosts().subscribe(posts => this.posts = posts);
```

## Lifecycle Hooks

```typescript
export class UserComponent implements OnInit, OnDestroy {
  ngOnInit() { /* after first render, init data */ }
  ngOnChanges() { /* when @Input changes */ }
  ngOnDestroy() { /* cleanup, unsubscribe */ }
}
```

## Routing

```typescript
const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'users/:id', component: UserComponent },
  { path: '**', component: NotFoundComponent },
];
```

```html
<a routerLink="/users/1">User</a>
<router-outlet></router-outlet>
```

## Pipes

```html
{{ price | currency:'USD' }}
{{ today | date:'short' }}
{{ name | uppercase }}
{{ text | slice:0:20 }}
{{ obj | json }}
```

---

[🔝 Back to README](../README.md)
