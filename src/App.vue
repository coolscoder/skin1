<template>
  <div id="data-container">
    <div id="information">
      <img data-avatar :src="linkdrip.profile.image" />
      <h1 data-username v-text="linkdrip.profile.name.text"></h1>
      <p data-description v-text="linkdrip.profile.description.text"></p>
    </div>
    <div id="drawhere" />
  </div>
</template>

<script>
  import {
    Engine,
    Render,
    World,
    Bodies,
  } from 'matter-js'

  export default {
    name: 'App',
    data: () => ({
      search: new URLSearchParams(window.location.search),
      visible: false,
      linktree: false,
      linkdrip:{
        skin: 'SKIN_NAME',
        profile:{
          image: "https://smhlancers.org/wp-content/uploads/2016/06/profile-placeholder-300x300.png",
          name:{
            text: "Webbendr"
          },
          description: {
            text: "This is my description"
          }
        },
        extends: [],
      }
    }),
    mounted: () => {
      let engine = Engine.create();
      let width = document.querySelector("#drawhere").offsetWidth
      let height = document.querySelector("#data-container").offsetHeight - document.querySelector("#information").offsetHeight - 50

      let render = Render.create({
        element: document.querySelector("#drawhere"),
        engine: engine,
        options: {
          width: width,
          height: height,
          wireframes: false,
        }
      });

      let boxA = Bodies.rectangle(width / 2, 200, 80, 80);
      let ballA = Bodies.circle(width / 2 - 10, 100, 40, 10);
      let ballB = Bodies.circle(width / 2 + 5, 10, 40, 10);
      let leftWall = Bodies.rectangle(1, height / 2, 1, height,  { isStatic: true,render: { fillStyle: 'white' }});
      let rightWall = Bodies.rectangle(width - 1, height / 2, 1, height,  { isStatic: true,render: { fillStyle: 'white' }});
      let ground = Bodies.rectangle(width / 2, height, height, 1,  { isStatic: true,render: { fillStyle: 'white' }});

      World.add(engine.world, [boxA, ballA, ballB, ground, leftWall, rightWall]);

      Engine.run(engine);

      Render.run(render);
    }
  }
</script>

<style lang="scss">
  body {
    color: #000;
    font-family: Helvetica;
    display: flex;
    justify-content: center;
  }
	#data-container {
    background-color: #fff;
    border-radius: 20px;
    width: 90vw;
    max-width: 300px;
    height: 100vh;
    padding-top: 20px;
	}
	[data-avatar]{
		position: relative;
		width: 100%;
		max-width: 4rem;
		border-radius: 100rem;
	}
	[data-username] {
		position: relative;
		font-size: 1.3rem;
		margin: 0 0 0.25rem;
	}
	[data-description]{
		position: relative;
		z-index: 5;
		font-size: 1rem;
	}
  #drawhere {
    canvas {
      background: #fff !important;
    }
  }
</style>
