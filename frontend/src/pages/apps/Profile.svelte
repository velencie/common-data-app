<script>
	import { quintOut } from 'svelte/easing';
    import { crossfade } from 'svelte/transition';

    let selected = 'Медицина';

	const [send, receive] = crossfade({
		duration: d => Math.sqrt(d * 200),

		fallback(node, params) {
			const style = getComputedStyle(node);
			const transform = style.transform === 'none' ? '' : style.transform;

			return {
				duration: 600,
				easing: quintOut,
				css: t => `
					transform: ${transform} scale(${t});
					opacity: ${t}
				`
			};
		}
	});

	let uid = 1;

	let tags = [
		{ id: uid++, description: 'Медицина' },
		{ id: uid++, description: 'Наука' },
		{ id: uid++, description: 'Бытовое' },
		{ id: uid++, description: 'Социальное' },
		{ id: uid++, description: 'Нейросети' },
	];

	function add() {
		const tag = {
			id: uid++,
			description: selected
		};

		tags = [tag, ...tags];
		selected = '';
	}

	function remove(tag) {
		tags = tags.filter(t => t !== tag);
	}
</script>

<main>
    <div class = "return"> 
        <p class="arrow">→</p>
        <div class="chaif">Главная</div>
    </div>
    <div class = "profile">
        <div class="main-block">
                <h2 class="pr">Профиль</h2>
                <a href="/" class="edit">Редактировать</a>   
        </div> 
        <div class="user_info">
            <div class="user">
                <img src="/images/user_images/user.jpg" class="photo" alt="user_photo"/>
                <h2 class="title"> Даня Драгун </h2>
            </div>
            <div class="self">
                <h2 class="title"> О себе </h2>
                <p> 23 y.o. designer from San Francisco </p>
            </div>
            <div class="catigories">
                <h2 class="title">Предпочтения</h2>
                <div class="tags">
                    {#each tags as tag (tag.id)}
                    <p class="tag-href"
                        in:receive="{{key: tag.id}}"
                        out:send="{{key: tag.id}}"
                    >
                        {tag.description}
                        <button on:click="{() => remove(tag)}" class="remove-btn">&times;</button>
                    </p>
                    {/each}
                    <select bind:value={selected}>
                        <option>Медицина</option>
                        <option>Наука</option>
                        <option>Бытовое</option>
                    </select>
                    <button on:click={() => add({selected})}></button>
                </div>
            </div>
            <div clsaa="exexperience">
                <h2 class="title">Участие в проектах:</h2>
                <div class="exexperience-tags">
                    <li>
                        <ul><a href="/" class="ex-tag">Карта ночного неба</a></ul>
                        <ul><a href="/" class="ex-tag">Влияние проходимого расстояния на здоровье</a></ul>
                        <ul><a href="/" class="ex-tag">Облысение. Исследование</a></ul>
                        <ul><a href="/" class="ex-tag">Длительность жизни обуви</a></ul>
                    </li>
                </div>
            </div>
        </div>
        <!--
        <div class="sections">
            <a href="/" class="sec-teg"><p>Информация</p></a>
            <a href="/" class="sec-teg"><p>Достижения</p></a>
        </div>
        -->
    </div>
</main>
<style>
    .profile {
        display: flex;
        flex-direction: row;
        align-items: flex-start;
    }
    .return {
        display: flex;
        flex-direction: row;
        align-items: flex-start;
        margin-top: 2%;
        margin-bottom: 2%;
    }

    main {
        margin-left: 5%;
    }
    /*main_block*/
    /*Привет пздц*/
    .arrow{ 

        font-family: "SF Pro Display";
        font-size: 32px;
        text-align: center;

        color: #282828;
        transform: rotate(-180deg);
        align-self: center;
        margin-right: 2%;
    }

    .chaif {
        position: static;
        width: 77px;
        height: 24px;
        left: 53px;
        top: 7px;

        /* just text */
        font-family: "Helvetica Norm";
        font-style: normal;
        font-weight: normal;
        line-height: 24px;
        /* identical to box height */
        display: flex;
        align-items: center;

        /* Inside Auto Layout */
        flex: none;
        order: 1;
        align-self: center;
        flex-grow: 0;
        margin: 16px 0px;
    }
    .edit {
        font-style: normal;
        font-weight: normal;
        line-height: 19px;
        margin-top: 2%;
        text-decoration-line: underline;
    }
    /*user-info*/
    .user_info {
        display: flex;
        flex-direction: column;
        margin-right: 15% ;
        margin-left: 25%;
    }

    .user {
        margin-bottom: 10%;
    }
    .self {
        margin-bottom: 10%;
    }
    .catigories {
        margin-bottom: 10%;
    }
    .tags {
        display: flex;
        align-items: center;
        margin-top: 3%;
        max-width: 10%;
    }  
    .close {
        margin-left: 4px;
        color: #F9F9F9;
        text-decoration: none;
        text-align: center;
    }

    .remove-btn {
        background-color: transparent;
        align-self: center;
        order: 1;
        font-size: 20px;
    }

    .remove-btn:focus {
        outline: none;
    }

    .tag-href {
        background-color: #282828;
        display: flex;
        align-items: flex-start;
        align-content: center;
        flex-direction: row;
        justify-content: center;
        font-size: 14px;
        border-radius: 18px;
        padding: 6px 0px 0px 12px;
        height: 30px;
        margin-right: 2%;
        margin-left: 0%;
        color: #F9F9F9;
    }
    .exexperience-tags {
        margin-top: 2%;
    }
    .ex-tag {
        text-decoration: none;
    }

    .photo {
        width: 128px;
        height: 128px;
        border-radius: 50%;
        margin-bottom: 1%;
    }

    li {
        list-style-type: none;
        margin-bottom: 30%;
    }

    ul {
        padding: 0;
        margin-block-start: 0em;
        margin-block-end: 0em;
        padding: 0;
        margin: 0;
        padding-inline-start: 0px;
    }

    /*
    .sections {
        display:flex;
        flex-direction: column;
    }
    .sec-teg {
        margin-top: 5%;
        text-decoration: none;
        font-size: 16px;
        line-height: 19px;
        color: #545454;
    }
    */

    @media (max-width: 768px) {
        .profile {
            flex-direction: column;
        }

        .user_info {
            margin-right: 0%;
            margin-left: 0%;
            max-width: 90%;
            margin-top: 5%;
        }
    }

</style>