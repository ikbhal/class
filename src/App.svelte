<script>
let cmd = "";
let messages = [];
function cmdHandler(e) {
	console.log("cmd handler e", e);
	if(e.key=="Enter"){
		processCmd(cmd);
	}
}

function processCmd(c) {
	console.log("process cmd c:",c);
	if(c.startsWith('/')){
		c = c.substring(1);
	}
	let parts = c.split(" ");
	let cmdName = '';
	if(parts && parts.length > 0){
		cmd = parts[0];
	}
	cmdName = cmdName.toLowerCase();
	switch(cmdName){
		case 'youtube':
			console.log("youtube video");
			break;
		case "tutorial":
			console.log("tutorial link, which language , framework, topic, category , tags");
			break;
		case 'img':
		case 'image':
			console.log("image handling");
			break;
		default:
			textCommand(c);
	}
}

function textCommand(c){
	console.log("inside text command");
	appendMessage(c);
}

function appendMessage(msg){
	messages.push(msg);
}
</script>

<main>
	<h1>Class</h1>
	
	<p>slack commands (tutorial, link , chat, youotube vide, image, screen shot,  github, vercel)</p>
	<p>class is considered as place, may be subject wise</p>
	
	<hr/>
	<form on:submit|preventDefault={() => console.log("form submitted")}>
		<input type="text" nam="cmd" bind:value={cmd} on:keyup={cmdHandler}>
	</form>
	<hr/>
	<div class="output">
		cmd: {cmd}
	</div>
	<div class="chat">
		{#each messages as msg}
		<div class="messsage">
		{msg}
		</div>
		{/each}

	</div>
</main>

<style>
</style>