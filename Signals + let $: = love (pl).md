# Signals + let $: = love

Chcę przekonać cię, że połączenie sygnałów ze Svelte 5 i składni reaktywnej `let` `$:` to najlepsze połączenie. I że jest jeszcze szansa uratować tę składnię bez żadnych strat.  
Although I know it will only be banging my head against the wall.

Ja osobiście byłem bardzo zahajpowany zapowiedziami Svelte 5. Dosłownie kocham Svelte a Rich Harris stał się dla mnie bogiem, gdy dowiedziałem się że jest autorem Svelte.  
Jednak prezentacja Svelte 5 spowodowała mieszane uczucia.

## Ważne dwa pytania

1. Czy zdarzyło ci się wewnątrz Svelte file użyć `let`, które nie byłoby ostatecznie reaktywne?
2. Czy zdarzyło ci się użyć czegoś reaktywnego, co nie byłoby `let`?

ad 1. Użycie `let element;` i `bind:this={element}` (i inne `bind`). Z tym że to jest w pewnym stopniu reaktywne.
ad 2. Użycie właściwości `const ob = {}`, które jest reaktywne w Svelte 3/4:

```svelte
<script>
  const ob = {
    a: 1,
    b: 2
  }
</script>
<button on:click={()=>ob.a++}>but</button>
{ob.a}
```

Uważam, że to jedyne przypadki.

Pierwszy przypadek nadal będzie działał w Svelte 5.

Drugi przypadek jest niemożliwy w Svelte 5.

Więc można uznać za prawdziwe stwierdzenie: "Nigdy nie użylibyśmy Run, w inny sposób niż używamy reaktywne let $:"

## Nigdy nie używamy let, które nie byłoby reaktywne

Na to zwrócił uwagę pomysłodawca [Pelte](https://pelte.dev/), w swoim [blogu](https://poxi.substack.com/p/my-thoughts-on-svelte-5-as-a-full?utm_source=profile&utm_medium=reader2) Filip Tangen.  
Polecam przeczytać jego argumenty.

Żeby nie tracić ŻADNYCH zalet Svelte 5, zaproponował zwyczajny preprocesor, który zamienia kod `let` `$:`, na runy ze Svelte 5.

![Pelte in action](pelte_in_action.png)

(pomijam kwestię `export let`, którą Pelte również się zajmuje)

To jest tak proste, i tak skuteczne.

## A co ze Svelte store?

Tutaj dochodzi moja mała cegiełka.

Napisałem [wpis](https://www.reddit.com/r/sveltejs/comments/16pvxkx/unification_using_runes_how_about_a_different_way/?utm_source=share&utm_medium=web2x&context=3) na Reddit, przedstawiający proste rozwinięcie pomysłu Pelte, o pliki js/ts.

```svelte
export /* @svelte */ function counter () {
  let count = 0;
  let double;
  $:double = count*2;
  // ...
}
```

Mając Svelte 5 i sygnały w nim zawarte, można z łatwością przekształcić powyższy kod w runy, tak samo jak Pelte przekształci kod pliku Svelte.  
Wcześniej, w Svelte 3/4 faktycznie nie byłoby to możliwe, więc Sygnały ze Svelte 5 są niezbędne. Więc dobrze, że w Svelte 5 są sygnały.

Każda inna funkcja działa jak zwykła funkcja js/ts. Reaktywność dotyczy tylko funkcji specjalnie oznaczone, w praktyce, tylko funkcji która jest kreatorem store.

Pod wpisem rozwinęła się mała dyskusja, w której rozwiały się wszelkie wątpliwości:

1. "Nie lubię komentarzy jako składni" - można użyć np. `export function $counter ()`, do oznaczania takich funkcji. To kwestia umowna.
2. "Runy można użyć na poziomie top level modułu" - jeśli to naprawdę przydatne, trzeba dać możliwość oznaczenia top level modułu jako reaktywnego. Oczywiście funkcje w module pozostają zwykłymi funkcjami, dopóki również jej nie oznaczysz jako reaktywnej - jest to na wzór oznaczania funkcji jako `async` czy też generatorów. Tak samo można zagnieżdżać funkcje reaktywne. Można robić z funkcjami reaktywnymi dokładnie to samo, co z runami!
3. "Brak zmiennych niereaktywnych spowoduje niezamierzone zachowanie efektów i pochodnych. Trudno śledzić, co się dzieje i dlaczego." - Nie. Jeśli nie oznaczysz tej funkcji jako reaktywnej, to jest to dokładnie zwykła funkcja. Nic złego nie może się stać.

Można by rozwinąć temat wątpliwości, i odpowiedzieć na nie, ale chciałbym najpierw przebić się z tematem w takiej formie, w jakiej jest.

W ten sposób mamy osiągnięty jeden z celów run - ten sam kod w pliku svelte, i w pliku js/ts.

## Uwagi

Nie każdy kod Svelte 3/4 będzie działał w Pelte. Kod Pelte, to nie jest dokładnie kod Svelte 3/4, bo korzysta z sygnałów.

## Podsumowanie

Boję się, że nie ma opcji, żeby wycofać się z run. Że nie ma sposobu, żeby zachować siłę let $:. Autoryter Richa jest zbyt silny, marketing wokół Svelte 5 był zbyt duży.
W tym sensie piszę ten wpis w beznadziei. Nie wierzę, że da się coś jeszcze zrobić, mimo że Svelte 5 tak naprawdę jeszcze nawet nie istnieje.

Nie chcę tracić SvelteKit, nie chcę tracić Svelte, nie chcę używać innego frameworka.

Zawsze można powiedzieć "wystarczy że Pelte naprawdę powstanie", ale to jest marne pocieszenie. Ciężko mi jest zaakceptować obecny stan.
