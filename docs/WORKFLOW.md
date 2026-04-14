# Workflow – Wasaty Ogrodnik (Hugo + Congo)

Ten plik opisuje krok po kroku, jak pracować nad stroną: dodawać posty, obrazki, miniaturki, galerię oraz jak używać Git.

Repozytorium:  
`D:/Dokumenty/Działka/Strona/wasatyogrodnik.github.io`

Motyw: **Congo** (posty jako page bundles).

---

bash
git status
git log --oneline -n 3
git remote -v

git add . && git commit -m "Poprawa widoku single i custom CSS" && git push origin main && git status && git log --oneline -n 3 && git remote -v

git add .
git commit -m "Poprawa widoku single i custom CSS"
git push origin main



## 0. Podstawowe komendy

```bash
# Wejście do projektu
cd "D:/Dokumenty/Działka/Strona/wasatyogrodnik.github.io"

# Podgląd lokalny
hugo server -D

# Build produkcyjny
hugo

# Git
git status
git add ...
git commit -m "Opis zmian"
git push
```

---

## 1. Struktura katalogów

Najważniejsze katalogi:

```text
content/
  posts/
    nazwa-posta/
      index.md        # treść posta (page bundle)
      feature.webp    # obrazek używany jako miniaturka/okładka posta
  galeria/
    _index.md         # strona z kafelkową galerią

static/
  images/
    galeria/
      grzadka.webp
      kompostownik-1.jpg
      ...
```

- Wszystkie „źródłowe” zdjęcia trzymamy w `static/images/galeria`.
- Do każdego posta kopiujemy **jedno wybrane zdjęcie** jako `feature.*` obok `index.md`.  
- Motyw Congo czyta `feature:` z front matter i robi z tego miniaturkę + cover.

---

## 2. Dodawanie i używanie zdjęć

### 2.1. Wrzucenie nowych zdjęć do galerii

```bash
cd "D:/Dokumenty/Działka/Strona/wasatyogrodnik.github.io"

mkdir -p static/images/galeria

cp "D:/skadś/grzadka.webp" static/images/galeria/grzadka.webp
cp "D:/skadś/kompostownik-1.jpg" static/images/galeria/kompostownik-1.jpg
```

Użycie w artykule (jeśli chcę w treści):

```markdown

```

---

## 3. Nowy post – pełny workflow

Przykład: post „Pryzma i dół kompostowy”.

### 3.1. Utworzenie bundla posta

```bash
hugo new posts/pryzma-i-dol-kompostowy/index.md
```

Powstanie:

```text
content/posts/pryzma-i-dol-kompostowy/index.md
```

### 3.2. Kopia obrazu do bundla jako `feature`

Zakładamy, że zdjęcie siedzi już w galerii:

```bash
cp static/images/galeria/pryzma.jpg \
   content/posts/pryzma-i-dol-kompostowy/feature.jpg
```

### 3.3. Front matter dla Congo

W `content/posts/pryzma-i-dol-kompostowy/index.md`:

```yaml
***
title: "Pryzma i dół kompostowy – który sposób wybrać na działce?"
date: 2026-04-15
draft: true
tags: ["kompost", "pryzma", "nawożenie", "ROD"]
categories: ["poradnik"]
feature: "feature.jpg"
***
```

- `feature: "feature.jpg"` – plik obok `index.md`.
- Congo użyje tego jako miniaturki na listach + okładki posta.

### 3.4. Treść posta

Pod front matter wklejam treść (Markdown).  
Jeśli chcę ten sam obraz w treści:

```markdown

```

### 3.5. Podgląd i publikacja

```bash
hugo server -D
```

- Sprawdzam `http://localhost:1313/posts/` – czy miniaturka jest.
- Sprawdzam `http://localhost:1313/posts/pryzma-i-dol-kompostowy/` – czy obraz w poście jest OK.

Gdy gotowe:

```yaml
draft: false
```

---

## 4. Zamiana starego posta na bundle

Jeśli istnieje post jako pojedynczy plik:

```text
content/posts/stary-post.md
```

### 4.1. Zrób z niego bundla:

```bash
mkdir -p content/posts/stary-post

mv content/posts/stary-post.md \
   content/posts/stary-post/index.md
```

### 4.2. Dodaj feature:

```bash
cp static/images/galeria/jakis-obraz.webp \
   content/posts/stary-post/feature.webp
```

W `index.md`:

```yaml
feature: "feature.webp"
```

---

## 5. Strona „Galeria” – kafelki + powiększanie

### 5.1. Katalog i plik

```bash
mkdir -p content/galeria
```

`content/galeria/_index.md`:

```markdown
***
title: "Galeria"
date: 2026-04-12
draft: false
***

<div class="grid grid-cols-2 md:grid-cols-3 gap-4">

  <a href="/images/galeria/grzadka.webp">
    <img src="/images/galeria/grzadka.webp" alt="Grządka na działce" class="rounded-lg" />
  </a>

  <a href="/images/galeria/kompostownik-1.jpg">
    <img src="/images/galeria/kompostownik-1.jpg" alt="Kompostownik" class="rounded-lg" />
  </a>

  <!-- kolejne obrazki w analogiczny sposób -->

</div>
```

- Kafelki generowane przez Tailwind (Congo).
- Po kliknięciu obraz otwiera się w pełnym rozmiarze.

---

## 6. Typowy dzień pracy – skrót

1. Start:

   ```bash
   cd "D:/Dokumenty/Działka/Strona/wasatyogrodnik.github.io"
   ```

2. Nowy post:

   ```bash
   hugo new posts/nazwa-posta/index.md
   ```

3. (Jeśli trzeba) wrzucenie nowego zdjęcia:

   ```bash
   cp "D:/skadś/zdjecie.webp" static/images/galeria/zdjecie.webp
   cp static/images/galeria/zdjecie.webp \
      content/posts/nazwa-posta/feature.webp
   ```

4. Edycja `index.md`:
   - ustawienie front matter (title/date/tags/categories/feature),
   - wklejenie treści artykułu.

5. Podgląd:

   ```bash
   hugo server -D
   ```

6. Zmiana `draft: false`, gdy gotowe.

7. Git:

   ```bash
   git status
   git add static/images/galeria content/posts/nazwa-posta
   git commit -m "Dodaj post: Tytuł posta"
   git push
   ```

---

## 7. Notatki

- Wszystkie zdjęcia **źródłowe** trzymaj w `static/images/galeria` – ułatwia to ich ponowne użycie.
- Do każdego posta kopiuj **tylko to jedno, które ma być „feature”**.
- Artykuły tworzysz zawsze jako **bundles** z `index.md`, żeby Congo bez kombinacji miało miniaturki.
