# Component Library Workshop

In this workshop, we'll build 3 components from scratch:

1. ProgressBar
2. Select
3. IconInput

Most of the pertinent information will be stored in the Figma document (https://www.figma.com/file/u0wCdLXheiN9f2FmAuPsE9/Mini-Component-Library), but this README will contain some additional information to help you on your mission!

Two fully-formed components have already been included, to be used as-needed in your work:

- `Icon`, an icon component that uses `react-feather` to render various icons
- `VisuallyHidden`, a component that allows us to make text available to screen-reader users, but not to sighted users.

Additionally, all of the colors you'll need are indexed in `constants.js`.

All components in this project use [the `Roboto` font](https://fonts.google.com/specimen/Roboto). This font is already included in the Storybook environment, and is already applied to all elements. It comes in two weights:

- 400 (default)
- 700 (bold)

## Running Storybook

This project uses Storybook, a component development tool.

First, install dependencies with `npm install`.

Once dependencies are installed, you can start storybook by running:

```
npm run start
```

Once running, you can visit storybook at http://localhost:6006.

## Troubleshooting

You may get an error when running the `start` script that looks like this:

```
Error: error:0308010C:digital envelope routines::unsupported
    at new Hash (node:internal/crypto/hash:67:19)
    at Object.createHash (node:crypto:130:10)
```

You can fix this issue either by downgrading to Node 16, or by updating the `package.json` file as follows:

```diff
  "scripts": {
-   "start": "start-storybook -p 6006 -s public",
+   "start": "NODE_OPTIONS=--openssl-legacy-provider start-storybook -p 6006 -s public",
  },
```

For more info, check out the [Troubleshooting Guide](https://courses.joshwcomeau.com/troubleshooting) on the course platform.

## The Components

### ProgressBar

The figma document mentions that this component should be "accessible". You can learn how to build a semantically-valid, accessible progress-bar component by reading this doc: https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_progressbar_role

This component uses a **box shadow**. We haven't seen this property yet! For now, you can achieve this effect by copying the following CSS declaration into your component:

```css
box-shadow: inset 0px 2px 4px ${COLORS.transparentGray35};
```

We'll learn much more about the `box-shadow` property in future modules =)

### Select

The Select component will need a down-arrow icon! You can use the `chevron-down` ID with the `Icon` component.

We want to use a native `<select>` tag in this component, so a bit of precursory HTML has been provided.

This component also includes a function, `getDisplayedValue`. This component uses some React APIs to work out the text that should be displayed. The value isn't currently used, but you can make use of it if needed, depending on your implementation.

> **NOTE:** Hiding the built-in chevron
>
> The `<select>` tag has a built-in chevron (downward-pointing
> character), but it's inconsistent between browsers. We want to
> use the provided “chevron-down” icon. But how do we replace the
> default built-in one??
>
> This is something we haven't learned how to do, and so you'll need
> to do some googling + experimentation. I want you to get a bit
> of practice trying to tackle “unconventional” challenges like this!
>
> If you get stuck and/or would like a hint, you'll find one on
> the solution page:
> https://courses.joshwcomeau.com/css-for-js/03-components/19-workshop-select#select

### IconInput

This component also uses the `Icon` component — the specific ID will be provided as a prop.

This component requires bold text. You can achieve this look by using `font-weight: 700`.

## Note to self

1. **Progress Bar**

   Three `<div>` elements - one the bar showing the 'progress', the other an outer wrapper in which we apply a `box-shadow` effect, and the last an inner 'trimmer', that trims the inner bar's overflow so that we get these nice rounded corners. There were also accessibility concerns covered with using `role='progressbar'` and some aria attributes. Also, dynamically styling our components using an object built outside the functional component, error handling, and using CSS variables.

2. **Select**

   Wild exercise. Here, Josh combines the native `<select>` with a 'presentational bit'. We get the nice form functionality, including the dropdown stuff which is both very nice across all browsers/OS's and is free! But we are not limited in terms of design.

   It's kinda like putting this fake but pretty looking sheen over a real `<select>`.

   Here's the structure:

   ```
   <Wrapper>
     <NativeSelect value={value} onChange={onChange}>
       {children}
     </NativeSelect>
     <PresentationalBit>
       {displayedValue}
       <IconWrapper style={{ '--size': 24 + 'px' }}>
         <Icon id='chevron-down' strokeWidth={1} size={24} />
       </IconWrapper>
     </PresentationalBit>
   </Wrapper>
   ```

   Other noteworthy aspects of this exercise:

   - `pointer-events: none` removes any potential pointer events from an element. For example, if you have an element that is layered on top and 'blocking' another element that does have a pointer event then you can remove this blocking by using this declaration.
   - Aligning absolutely positioned elements centrally along the vertical trick: set `top` and `bottom` to 0, and `margin: auto`. Requires dimensions.
   - Setting `opacity: 0` achieves the effect of `display: none` with the important difference that users can still interact with the element. In this case, this fit the bill.
   - Next sibling combinator. e.g.

     ```
     img + p {
      font-weight: bold;
     }
     ```

     will target any `<p>` element that _directly_ follows an `<img>` element.

     If you wanted to target _any_ subsequent sibling, we use `~` instead of `+`:

     ```
     img ~ p {
       color: red;
     }
     ```

     In the context of this exercise, the next sibling combinator was used like this:

     ```
     const PresentationalBit = styled.div`
       ...

       ${NativeSelect}:hover + & {
         color: ${COLORS.black};
       }
     ```

     So this is effectively adding the rule:

     ```
     select:hover + div {
       color: ...
     }
     ```

     which targets any `<div>` that directly follows a hovered `<select>` (including the styled-component class names that are generated, of course).

     Worth noting that in styled-components, `&` refers to all instances of the component.

3. **IconInput**

   This was a bit more straightforward. I suppose a couple of noteworthy aspects were how the `<label>` wrapped the `<input>`, and how we can target the placeholder text with the `::placeholder` pseudo element.
