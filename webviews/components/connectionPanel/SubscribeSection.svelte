<script lang="ts">
    import { ColorManager } from '.././ColorManager';
    import { subscriptions } from '.././stores';

    let subscribeTopic: string;
    let selectedQos: string = "0";

    function subscribe() {
        if (!subscribeTopic) {
            return;
        }

        vscode.postMessage({
            type: "subscribe",
            value: {
                topic: subscribeTopic,
                qos: parseInt(selectedQos),
            },
        });
        
        $subscriptions = $subscriptions.set(
            subscribeTopic, 
            { 
                topic: subscribeTopic, 
                qos: parseInt(selectedQos), 
                color: ColorManager.getColor(subscribeTopic),
                messageCount: 0
            }
        );

        subscribeTopic = "";
    }
</script>

<h2>Subscribe</h2>
<form on:submit|preventDefault={subscribe}>
    <div class="subscription-options">
        <input type="text" bind:value={subscribeTopic} placeholder="Topic" />
        <select bind:value={selectedQos}>
            <option value="0">QoS 0</option>
            <option value="1">QoS 1</option>
            <option value="2">QoS 2</option>
        </select>
    </div>
    <button type="submit">Subscribe</button>
</form>

<style>
    .subscription-options {
        display: flex;
        height: 30px;
        margin-bottom: 1px;
        margin-top: 5px;
    }

    input {
        width: 50%;
        margin-right: 5px;
    }

    select {
        margin-left: 5px;
    }
    
    button {
        margin-bottom: 0;
    }
</style>
