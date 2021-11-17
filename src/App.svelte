<script>
import { text } from "svelte/internal";
import { writable } from "svelte/store";

let cmd = "";
let messages = [];
class Tutorial {
	constructor(id, text, link){
		this.id = id;
		this.text = text;
		this.link = link;
	}
}

let tutorialStore = writable([]);
let showTutorial = false;
let tutorialCount = 0;

//youtube 
let youtubeStore = writable([]);
let showYoutube = false;
let ytCount = 0;
class Youtube {
	constructor(id, text, link){
		this.id =id;
		this.text = text;
		this.link = link;
	}
}
let yt = new Youtube("", "", "");

function addTutorial(text, link){
	console.log('add tutorial');
	let tutorial = new Tutorial(tutorialCount++, text, link);
	$tutorialStore = [... $tutorialStore, tutorial];
}

function toggleTutorial(){
	showTutorial = !showTutorial;
}

// youtube 
function addYoutube(link, text){
	console.log("inside add youtube");
	yt = createYoutube(link, text);
	$youtubeStore = [... $youtubeStore, yt];
}

function createYoutube(link, text){
	return new Youtube(ytCount++, text, link);
}

function toggleYoutube(){
	showYoutube = !showYoutube;
}

function deleteYoube(id) {
	console.log("delete youtube");
}

function cmdHandler(e) {
	console.log("cmd handler e", e);
	if(e.key=="Enter"){
		processCmd(cmd);
	}
}

function processCmd(c) {
	console.log("process cmd c:",c);
	if(c.trim() == ""){
		return;
	}
	if(c.startsWith('/')){
		c = c.substring(1);
	}
	let parts = c.split(" ");
	let operation = '';
	let cmdName = '';
	let link = '';
	let text = '';
	if(parts && parts.length > 0){
		cmdName = parts[0];
	}
	cmdName = cmdName.toLowerCase();
	switch(cmdName){
		case 'youtube': // command: /yt add link text words
		case 'yt':
			operation = parts[1];
			if(operation == "add"){
				link = parts[2];
				text = parts.join(" "); 	
				console.log("youtube video");
				addYoutube(link, text);
			}
			break;
		case "tutorial": // command: /tutorial add singlewordtext link
			console.log("tutorial link, which language , framework, topic, category , tags");
			if(parts[1] == "add"){
				addTutorial(parts[2], parts[3]);
			}else if(parts[1]== "list"){
				showTutorial = true;
			}
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

function deleteTutorial(id){
	console.log("delete tutorial id: ", id);
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

	<button on:click={toggleTutorial}>Toggle Tutorial</button><br>
	<div class="tutorials">
		{#each $tutorialStore as tutorial}
		<div class="tutorial">
			<a href={tutorial.link}> {tutorial.text} </a>
			<button on:click={deleteTutorial(tutorial.id)}>Delete</button>
		</div>
		{/each}
	</div>

	<!-- youtube -->
	<button on:click={toggleYoutube}>Toggle Youtube</button><br>
	<div class="youtubeVideos">
		{#each $youtubeStore as youtube}
		<div class="youtube">
			<a href={youtube.link}> {youtube.text} </a>
			<button on:click={deleteYoutube(youtube.id)}>Delete</button>
		</div>
		{/each}
	</div>

</main>

<style>
</style>