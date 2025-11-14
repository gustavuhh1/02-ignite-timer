# Ignite Timer ⏱️

Este projeto é um cronômetro desenvolvido em React com TypeScript e Vite, criado como parte do aprendizado de novos hooks do React, como `useEffect` e `useContext`. O projeto foi desenvolvido com base nos cursos da [Rocketseat](https://www.rocketseat.com.br/).

## 🚀 Tecnologias

- [React](https://reactjs.org/)
- [TypeScript](https://www.typescriptlang.org/)
- [Vite](https://vitejs.dev/)
- [Styled Components](https://styled-components.com/)
- [React Hook Form](https://react-hook-form.com/)
- [Zod](https://zod.dev/)
- [Immer](https://immerjs.github.io/immer/)

## 📚 Aprendizados

### useEffect

O hook `useEffect` foi utilizado para realizar efeitos colaterais em componentes funcionais. Um exemplo de uso está no componente [`Countdown`](src/pages/Home/components/Countdown/index.tsx):

```tsx
import { useContext, useEffect } from "react";
import { CountContainer, Separator } from "./styles";
import { differenceInSeconds } from "date-fns/differenceInSeconds";
import { CyclesContext } from "../../../../contexts/CyclesContext";

export function Countdown() {
  const { activeCycle, activeCycleId, markCurrentCycleAsFinish, amountSecondsPassed, setSecondsPassed } =
    useContext(CyclesContext);

  const totalSeconds = activeCycle ? activeCycle.minutesAmount * 60 : 0;

  useEffect(() => {
    let interval: number;

    if (activeCycle) {
      interval = setInterval(() => {
        const secondsDifference = differenceInSeconds(new Date(), new Date(activeCycle.startDate));

        if (secondsDifference >= totalSeconds) {
          markCurrentCycleAsFinish();
          setSecondsPassed(totalSeconds);
          clearInterval(interval);
        } else {
          setSecondsPassed(secondsDifference);
        }
      }, 1000);
    }

    return () => {
      clearInterval(interval);
    };
  }, [activeCycle, totalSeconds, activeCycleId, markCurrentCycleAsFinish, setSecondsPassed]);

  const currentSeconds = activeCycle ? totalSeconds - amountSecondsPassed : 0;

  const minutesAmount = Math.floor(currentSeconds / 60);
  const secondsAmont = currentSeconds % 60;

  const minutes = String(minutesAmount).padStart(2, "0");
  const seconds = String(secondsAmont).padStart(2, "0");

  useEffect(() => {
    if (activeCycle) {
      document.title = `${minutes}:${seconds}`;
    }
  }, [minutes, seconds, activeCycle]);

  return (
    <CountContainer>
      <span>{minutes[0]}</span>
      <span>{minutes[1]}</span>
      <Separator>:</Separator>
      <span>{seconds[0]}</span>
      <span>{seconds[1]}</span>
    </CountContainer>
  );
}
```

### useContext

O hook useContext foi utilizado para compartilhar estado entre componentes sem a necessidade de passar props manualmente em cada nível da árvore de componentes. Um exemplo de uso está no componente **NewCycleForm**:

```tsx
import { CyclesContext } from "../../../../contexts/CyclesContext";
import { FormContainer } from "./styles";
import { MinutesAmountInput, TaskInput } from "./styles";

import { useContext } from "react";
import { useFormContext } from "react-hook-form";

export function NewCycleForm() {
  const { activeCycle } = useContext(CyclesContext);
  const { register } = useFormContext()

  return (
    <FormContainer>
      <label htmlFor="task">Vou trabalhar em</label>
      <TaskInput
        type="text"
        list="task-suggestions"
        id="task"
        placeholder="Dê um nome para o seu projeto"
        disabled={!!activeCycle}
        {...register("task")}
      />

      <datalist id="task-suggestions">
        <option value="Projeto 1" />
        <option value="Projeto 2" />
        <option value="Projeto 3" />
        <option value="fruta" />
      </datalist>

      <label htmlFor="minutesAmount">durante</label>
      <MinutesAmountInput
        type="number"
        id="minutesAmount"
        placeholder="00"
        step={5}
        min={5}
        max={60}
        disabled={!!activeCycle}
        {...register("minutesAmount", { valueAsNumber: true })}
      />

      <span>minutos.</span>
    </FormContainer>
  );
}
```

### 📂 Estrutura do Projeto

.gitignore
eslint.config.js
index.html
package.json
public/
README.md
src/
    @types/
        styled.d.ts
    App.tsx
    assets/
    components/
        Header/
            index.tsx
            styles.ts
    contexts/
        CyclesContext.tsx
    layouts/
        DefaultLayout/
            index.tsx
            styles.ts
    main.tsx
    pages/
        History/
            index.tsx
            styles.ts
        Home/
            components/
            index.tsx
            styles.ts
    reducers/
        cycles/
    Router.tsx
    styles/
        global.ts
        themes/
    vite-env.d.ts
tsconfig.app.json
tsconfig.json
tsconfig.node.json
vite.config.ts

### 🛠️ Como Executar

Para executar o projeto, siga os passos abaixo:

1. Clone o repositório:

```bash
git clone https://github.com/gustavuhh1/02-ignite-timer.git
```

2. Instale as dependências

```bash
npm install
```

3. Execute o projeto em modo de desenvolvimento:

```bash
npm run dev
```

## 📜 Licença

Este projeto está licenciado sob a licença MIT. Veja o arquivo LICENSE para mais detalhes.

Feito com 💜 por [Gustavo Martins Rodrigues](https://github.com/gustavuhhh1) durante os cursos da **Rocketseat**.
