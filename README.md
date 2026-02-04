# iconis

[![npm version](https://img.shields.io/npm/v/@iconis/icons)](https://www.npmjs.com/package/@iconis/icons)
[![npm downloads](https://img.shields.io/npm/dw/@iconis/icons)](https://www.npmjs.com/package/@iconis/icons)
[![license](https://img.shields.io/npm/l/@iconis/icons)](https://www.npmjs.com/package/@iconis/icons)

Iconis é uma biblioteca de ícones em **SVG inline** para uso em projetos web.

- <u>**Sem webfont:**</u> evita o efeito de “texto virando ícone” causado por atraso de carregamento de fontes ou bundles pesados.
- <u>**Leve e previsível:**</u> os ícones são simples strings SVG, sem runtime, sem observers e sem dependências extras.
- <u>**Tree-shaking friendly:**</u> cada ícone é exportado individualmente, permitindo que bundlers modernos incluam apenas os ícones realmente utilizados.
- <u>**Excelente para projetos legados ou sensíveis à performance:**</u> não depende de loaders, fontes externas ou inicialização assíncrona — ideal para aplicações antigas ou ambientes com restrições de bundle.
- <u>**Controle total de estilo:**</u> ícones com `fill="currentColor"` permitem controle de cor via CSS ou Tailwind.
- <u>**Exports tipados e documentados com TSDoc:**</u> autocomplete e tooltips claros diretamente no editor, facilitando a implementação.


## Instalação

```bash
npm install @iconis/icons
```

## Uso
A biblioteca suporta dois estilos de import, pensados para atender **projetos modernos** e **projetos legados**.


### ✅ Projetos modernos (Angular 16+, Angular17+, Vite, etc)
Recomendado para projetos novos ou projetos que utilizam:
- Typescript recente
- Com suporte a **subpath exports** (`moduleResolution: "bundler"`)

```ts
import { starRegular } from '@iconis/icons/outlined';
import { cakeRegular } from '@iconis/icons/filled';
```


### ⚠️ Projetos legados (Angular 14- / typescript antigo)
Em projetos mais antigos (como Angular 14), o TypeScript **não resolve corretamente subpath exports** sem ajustes adicionais de configuração.

Nesses casos, utilize o import via **namespace**, que é totalmente compatível:

```ts
import { outlined, filled } from '@iconis/icons';

const stariIconOutline = outlined.starRegular;
const starIconFilled = filled.starRegular;
```
Esse formato funciona **sem necessidade de alterar** o `tsconfig` do projeto.


### ℹ️ Sobre o tsconfig e `moduleResolution: "bundler"`
É possível habilitar o import moderno (`@iconis/icons/outlined`) em projetos mais antigos ajustando o `tsconfig.json`:

```ts
{
  "compilerOptions": {
    "moduleResolution": "bundler"
  }
}
```
>**Atenção:** <br/>
>Essa mudança afeta a resolução de módulos do projeto inteiro.<br/>
>É recomendada **apenas para projetos novos ou bem controlados**.<br/>
>Em projetos legados grandes, o uso do import via namespace costuma ser mais seguro.


### Exemplo de uso no Angular

#### Angular 17+ (com `inject` moderno)

```ts
import { Component, inject } from '@angular/core';
import { DomSanitizer, SafeHtml } from '@angular/platform-browser';
import { cakeRegular } from '@iconis/icons/outlined';

@Component({
  selector: 'app-exemplo',
  standalone: true,
  template: `<span class="w-6 h-6 text-slate-600" [innerHTML]="icone"></span>`,
})
export class ExemploComponent {
  private readonly sanitizer = inject(DomSanitizer);

  protected readonly icone: SafeHtml =
    this.sanitizer.bypassSecurityTrustHtml(cakeRegular);
}
```

#### Angular versões anteriores (injeção via constructor)

```ts
import { Component } from '@angular/core';
import { DomSanitizer, SafeHtml } from '@angular/platform-browser';
import { outlined } from '@iconis/icons';

@Component({
  selector: 'app-exemplo',
  template: `<span class="w-6 h-6 text-slate-600" [innerHTML]="icone"></span>`,
})
export class ExemploComponent {
  public readonly icone: SafeHtml;

  constructor(private readonly sanitizer: DomSanitizer) {
    this.icone = this.sanitizer.bypassSecurityTrustHtml(outlined.cakeRegular);
  }
}
```
>**Nota:** <br/>
>Por ser SVG inline (string), de modo geral basta importar o icone, atrelar a uma variavel (para ser usado no template html) e Sanitizar para aplicar no `innerHTML`.


### Uso em projetos Javascript puro (HTML + JS)
O Iconis **não depende de Angular ou TypeScript**.  
Como os ícones são apenas **strings SVG**, eles podem ser usados em qualquer projeto JavaScript simples.

#### Exemplo básico (HTML + JS)
```html
<div id="icone"></div>

<script type="module">
  import { outlined } from '@iconis/icons';

  const container = document.getElementById('icone');
  container.innerHTML = outlined.starRegular;
</script>
```

#### Observações importantes
- Em projetos simples, `innerHTML` pode ser usado diretamente.
- Em aplicações maiores ou com conteúdo dinâmico, sanitize o HTML antes de inserir para evitar riscos de segurança.
- O uso de `type="module"` é necessário para imports ES modernos.

#### Exemplo com controle de cor via CSS
```ts
<style>
  .icon {
    width: 24px;
    height: 24px;
    color: #0f172a; /* slate-900 */
  }
</style>

<div id="icone" class="icon"></div>

<script type="module">
  import { outlined } from '@iconis/icons';

  document.getElementById('icone').innerHTML = outlined.starRegular;
</script>
```

Como os ícones utilizam `fill="currentColor"`, a cor é herdada automaticamente via CSS.


## Changelog
Veja o histórico de mudanças entre versões em [CHANGELOG.md](./CHANGELOG.md)


## Licença
MIT © [Iconis](https://github.com/Pdro-marqss/iconis-lib)