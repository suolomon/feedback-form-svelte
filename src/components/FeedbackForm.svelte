<script>
    import {v4 as uuidv4} from 'uuid'
    import {FeedbackStore} from '../stores'
    import Card from './Card.svelte';
    import Button from './Button.svelte'
    import RatingSelect from './RatingSelect.svelte';

    let text = ''; 
    let btnDisabled = true;
    let min = 10
    let message
    let rating = 10

    const handleSelect = e => rating = e.detail

    const handleInput = () => {
        if(text.trim().length <= min ) {
            message = `Text should at least be ${min} characters`
            btnDisabled = true
        } else {
            message = null 
            btnDisabled = false
        }
    }
    const handleSubmit = () => {
        if(text.trim().length > min) {
            const newFeedback = {
                id: uuidv4(),
                text,  
                rating: +rating
            }
            FeedbackStore.update((currentFeedback) =>{
              return [newFeedback, ...currentFeedback]
            } )

            text = ''
        } 
    }
</script>

<form on:submit|preventDefault={handleSubmit}>
    <Card>
        <RatingSelect on:rating-select={handleSelect}/>
        <header>Rate your experience with Svelte</header>
        <div class="input-group">
            <input type="text" on:input={handleInput} bind:value ={text} placeholder="Tell us something!">
            <Button disabled={btnDisabled} type="submit">Send</Button>
        </div>
        {#if message}
        <div class="message">{message}</div>
        {/if}
    </Card>
</form>

<style>
  header {
    max-width: 400px;
    margin: auto;
  }
  .input-group {
    display: flex;
    flex-direction: row;
    border: 1px solid #ccc;
    padding: 8px 10px;
    border-radius: 8px;
    margin-top: 15px;
  }
  input {
    flex-grow: 2;
    border: none;
    font-size: 16px;
  }
  input:focus {
    outline: none;
  }
  .message{
    padding-top: 10px;
    text-align: center;
    color: rebeccapurple;
  }
</style>