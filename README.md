![ngx](https://user-images.githubusercontent.com/8418700/143725233-c5b3320c-30ab-4077-9939-9b37dd6be5db.png)

## Problem

Angular does not support [optional host-element](https://github.com/angular/angular/issues/18877) officially so we should stick to annoying `div` which sometimes destroys our component design especially for 3rd party CSS frameworks like `Bootstrap`.

## Solution

There is a trick to achieve a `no-host` component:

```typescript
import {
  AfterViewInit,
  Component,
  ElementRef,
  OnInit,
  TemplateRef,
  ViewChild,
  ViewContainerRef
} from '@angular/core';

@Component({
  selector: 'my-nohost-component',
  // Keep the same structure is necessary. 
  // Put any component's detail inside 'ng-template'.
  // You can use 'templateUrl' too but with the same structure.
  template: '<ng-template #nohost>HERE!</ng-template>',
})
export class MyNoHostComponent implements OnInit, AfterViewInit {
  constructor(
    private readonly element: ElementRef,
    private readonly viewContainer: ViewContainerRef
  ) {}
  
  @ViewChild('nohost', { static: true }) noHostRef: TemplateRef<{}> | undefined;

  ngOnInit(): void {
    if (this.noHostRef)
        this.viewContainer.createEmbeddedView(this.noHostRef);
  }
  ngAfterViewInit(): void {
    this.element.nativeElement.remove();
    /* Another way
      document
        .querySelectorAll(this.elem.nativeElement.tagName.toLowerCase())
        .forEach((el) => el.parentNode.removeChild(el));
    */
  }
}
```

![Screenshot](https://user-images.githubusercontent.com/8418700/143724048-93872d5e-b634-4687-9bcd-8edfc610abbc.png)

### Usage

![](https://vsmarketplacebadge.apphb.com/version/hamedfathi.angular-nohost.svg)
![](https://vsmarketplacebadge.apphb.com/installs/hamedfathi.angular-nohost.svg)
![](https://vsmarketplacebadge.apphb.com/downloads/hamedfathi.angular-nohost.svg)
![](https://vsmarketplacebadge.apphb.com/rating/hamedfathi.angular-nohost.svg)

This snippet code `ngx-noHost` helps you write your `no-host` components faster. [Install from here](https://marketplace.visualstudio.com/items?itemName=hamedfathi.angular-nohost). 





