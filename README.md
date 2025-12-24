import { Injectable } from '@angular/core';
import {
  HttpEvent,
  HttpInterceptor,
  HttpHandler,
  HttpRequest,
  HttpResponse
} from '@angular/common/http';
import { Observable, tap } from 'rxjs';

@Injectable()
export class SignatureInterceptor implements HttpInterceptor {

  intercept(
    req: HttpRequest<any>,
    next: HttpHandler
  ): Observable<HttpEvent<any>> {

    return next.handle(req).pipe(
      tap(event => {
        if (event instanceof HttpResponse) {

          const signature = event.headers.get('x-signature');
          const timestamp = event.headers.get('x-timestamp');

          if (signature && timestamp) {
            sessionStorage.setItem('x-signature', signature);
            sessionStorage.setItem('x-timestamp', timestamp);
          }
        }
      })
    );
  }
}
