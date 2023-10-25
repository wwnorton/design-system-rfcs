- Start Date: 2023-10-25
- Related documents:
   - [Visual Design](https://app.zeplin.io/project/5d66e28439bbe3139aa846ad/screen/63a3267bff9fb3333a90b52c)
   - [Docs](https://docs.google.com/document/d/1G-IQyi3Sm5z-0Csk6Yv_jWLfoZd0airP04z0FBC-Hyw/edit#heading=h.c5hzr2347uf7)
- RFC PR: 

## Summary

*Multiple Choice Component* includes a combination of currently existing NDS components that facilitates the implementation
of the [Multiple Choice Pattern](https://app.zeplin.io/project/5d66e28439bbe3139aa846ad/screen/63a3267bff9fb3333a90b52c).

It also includes a *Feedback Modal* Sub-component which is simply a Modal with a structure optimized for rendering
the feedback of a selected choice.

- `<MultipleChoice>` - wraps up Radio Group and includes question intro, stem and Response Indicator.
- `<MultipleChoice.FeedbackModal>` - This modal renders the feedback for the selected choice, alongside the choice text.

## Detailed design

[TODO]

### Multiple Choice Component

```ts
// TODO: this might extend from radio group
interface MultipleChoiceProps {
   /** The ID of the selected choice */
   selected?: string;

   /** This is called whenever the user selects a new option */
   onChange?: (choiceId: string) => void;

   /**
    * The label type for the choices.
    * @default "letters"
    */
   choiceLabelType?: "roman" | "number" | "letters";

   /**
    * If the Response Indicator shouldn't be render
    * @default false
    */
   noIndicator?: boolean;

   /**
    * Introductory content that helps frame the question.
    */
   intro?: React.ReactNode;

   /**
    * The prompt the user responds to
    */
   stem: React.ReactNode;
   
   /**
    * If the choices should be shuffled.
    * When enabled, shuffling happens at component mount.
    * @default false
    */
   shuffle?: boolean;

   /**
    * If the selected choice is correct.
    */
   isCorrect: boolean | undefined;

   /**
    * The choices available for question
    */
   choices: Array<{
      /**
       * The ID of the choice, this is used to match against `isCorrect`.
       */
      id: string;

      /**
       * The Feedback of the choice to be displayed in the feedback modal
       * after the user submits the answer.
       */
      feedback: React.ReactNode;

      /**
       * The content of the choice.
       */
      body: React.ReactNode
   }>;
}
```

#### Example

```tsx
export function MultipleChoicePattern() {
   const [selected, setSelected] = useState(undefined);
   const [isCorrect, setIsCorrect] = useState(undefined);

   const onSelect = useCallback((choiceId) => {
      setSelected(choiceId);
   })

   const onSubmit = useCallback(() => {
      if (selected === '1') {
         setIsCorrect(true);
      } else {
         setIsCorrect(false);
      }
   });

   return (
      <form onSubmit={onSubmit}>
         <MultipleChoice
            selected={selected}
            onChange={onSelect}
            intro={<div>Lorem ipsum dolor sit amet</div>}
            stem="Lorem ipsum dolor sit amet, consectetur adipiscing elit. Phasellus tempor nisi lacus?"
            isCorrect={isCorrect}
            choices={
               [
                  { id: '1', feedback: 'F1', body: 'Answer 1' },
                  { id: '2', feedback: 'F2', body: 'Answer 2' },
                  { id: '3', feedback: <div>ASD</div>, body: <div>Answer 3</div> },
               ]
            }
         >
            <MultipleChoice.FeedbackModal />
         </MultipleChoice>
         <button type="submit"/>
      </form>
   )
}
```

## Drawbacks

[TODO]

## Alternatives

Custom Feedback Modal implemented as **Render Prop**:

```tsx
<MultipleChoice
   feedbackModal={(props) => <Modal {...props} />}
/>
```

Choices can also be implemented as sub-components:

```tsx
<MultipleChoice
   selected={selected}
   onChange={onSelect}
   intro={<div>Lorem ipsum dolor sit amet</div>}
   stem="Lorem ipsum dolor sit amet, consectetur adipiscing elit. Phasellus tempor nisi lacus?"
   isCorrect={isCorrect}
>
   {/* Extends Radio Props */}
   <MultipleChoice.Choice choiceId="1" feedback="F1">Choice 1</MultipleChoice.Choice>
   <MultipleChoice.Choice choiceId="2" feedback="F2">Choice 2</MultipleChoice.Choice>
   <MultipleChoice.Choice choiceId="3" feedback="F3">Choice 3</MultipleChoice.Choice>
</MultipleChoice>
```

## Adoption strategy

[TODO]

## Unresolved questions

[TODO]
