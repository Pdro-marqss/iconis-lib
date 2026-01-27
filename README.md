# iconis

[![npm version](https://img.shields.io/npm/v/@iconis/icons)](https://www.npmjs.com/package/@iconis/icons)
[![npm downloads](https://img.shields.io/npm/dw/@iconis/icons)](https://www.npmjs.com/package/@iconis/icons)
[![license](https://img.shields.io/npm/l/@iconis/icons)](https://www.npmjs.com/package/@iconis/icons)

Iconis é uma biblioteca de ícones em **SVG inline** para uso em projetos web.
- Sem webfont - Evita “texto virando ícone” do nada por atraso de carregamento ou peso do bundle
- Ícones com `fill="currentColor"` - Possibilita controlar cor via CSS/Tailwind;
- Exports tipados e documentados usando TSDocs - Disponibilizando tooltip amigavel que ajuda na hora de implementar no código;

## Instalação
No terminal do projeto:

```bash
npm install @iconis/icons
```
## Uso
### Exemplo de uso em Angular 17+ (com inject)
Em versões mais novas do angular temos o inject moderno disponivel, sem necessidade de usar o construtor.

```ts
import { Component, inject } from '@angular/core';
import { DomSanitizer, SafeHtml } from '@angular/platform-browser';
import { cakeIconRegular } from '@iconis/icons';

@Component({
  selector: 'app-exemplo',
  standalone: true,
  template: `<span class="w-6 h-6 text-slate-600" [innerHTML]="icone"></span>`,
})
export class ExemploComponent {
  private readonly sanitizer = inject(DomSanitizer);

  protected readonly icone: SafeHtml =
    this.sanitizer.bypassSecurityTrustHtml(cakeIconRegular);
}
```

### Exemplo de uso em versões anteriores do Angular (com constructor)
Em versões mais antigas do Angular precisamos injetar através do construtor, do modo tradicional.

```ts
import { Component } from '@angular/core';
import { DomSanitizer, SafeHtml } from '@angular/platform-browser';
import { cakeIconRegular } from '@iconis/icons';

@Component({
  selector: 'app-exemplo',
  template: `<span class="w-6 h-6 text-slate-600" [innerHTML]="icone"></span>`,
})
export class ExemploComponent {
  public readonly icone: SafeHtml;

  constructor(private readonly sanitizer: DomSanitizer) {
    this.icone = this.sanitizer.bypassSecurityTrustHtml(cakeIconRegular);
  }
}
```
> **Nota:** Por ser SVG inline (string), de modo geral basta importar o icone, atrelar a uma variavel (para ser usado no template html) e Sanitizar para aplicar no `innerHTML`

## Licença
MIT © [Iconis](https://github.com/Pdro-marqss/iconis-lib)