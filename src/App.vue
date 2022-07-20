<template>
  <div class="wrapper">
    <div class="textAreaWrapper">
      <div class="textArea" id="chatArea" ref="textContainer">
        <TextBox className="text1">Hey there 👋</TextBox>
        <TextBox className="text2">I'm Mayank</TextBox>
        <TextBox className="text3">I design and code things on web</TextBox>
        <TextBox className="text4"
          >I m currently looking for an internship</TextBox
        >
        <TextBox className="text5"
          >You have questions for me? Feel free to ask..</TextBox
        >
        <TextBox className="text6" v-bind:sender="false"
          >Have a nice day ✨</TextBox
        >
      </div>
    </div>
    <div ref="controlArea" class="controlArea">
      <div class="sendControls">
        <input
          type="text"
          name=""
          id="message"
          v-model="message"
          placeholder="Message"
          ref="messageBox"
        />
        <button @click="aboutMe">About You?</button>
        <button @click="resume">Resume?</button>
        <button @click="yourWork">Your works?</button>
        <button @click="contact">Contact?</button>
        <button @click="sendText">Send</button>
      </div>
    </div>
  </div>
</template>

<script>
import TextBox from "./components/TextBox.vue";
import { createApp, h } from "vue";
import { animate, timeline } from "motion";

export default {
  components: { TextBox },
  data() {
    return {
      myWork: false,
      nodeNumber: 8,
      message: "",
    };
  },
  methods: {
    async addNewTextMessage(message, className) {
      const newText = document.createElement("div");
      const container = this.$refs.textContainer;
      if (className) {
        newText.classList.add(`${className}`);
      }
      newText.classList.add(`text${this.nodeNumber}`);
      newText.innerHTML = `<p>${message}</p>`;
      container.appendChild(newText);

      newText.scrollIntoView();
      container.scrollTop = newText.scrollHeight;

      let length = message.length;
      let duration = 1;
      if (length < 10) {
        length = 10;
        duration = length * 0.01 + 1;
      }
      if (length > 25) {
        length = length - 5;
        duration = 2;
      }
      const finishAnimation = await animate(
        `.text${this.nodeNumber}`,
        {
          opacity: 1,
          transform: ["scale(0.1)", "translateY(-10px) scale(1)"],
        },
        { endDelay: duration },
        { duration: 0.3 },
        { easing: "ease-in-out" }
      ).finished;
      this.nodeNumber = this.nodeNumber + 1;
      return finishAnimation;
    },

    async aboutMe() {
      this.$refs.controlArea.style.opacity = "0.7";

      let nodes = this.$refs.controlArea.getElementsByTagName("*");
      for (var i = 0; i < nodes.length; i++) {
        nodes[i].disabled = true;
      }

      const messages = [
        "Currently I m pursuing BTech degree in Computer Science and Engineering",
        "I m interested in web development and competitive coding",
        "I also love to read books about psychology",
      ];
      await this.addNewTextMessage("Tell me more about you", "senderMessage");
      for (let message of messages) {
        const animation = await this.addNewTextMessage(message, "message");
      }

      this.$refs.controlArea.style.opacity = "1";
      for (var i = 0; i < nodes.length; i++) {
        nodes[i].disabled = false;
      }
    },

    async renderProject(details) {
      // destruct details object
      const { title, source, description, repo, preview } = details;
      // create a new div
      const newProjectContainer = document.createElement("div");
      newProjectContainer.classList.add("message");
      newProjectContainer.classList.add(`text${this.nodeNumber}`);
      // set inner html
      newProjectContainer.innerHTML = `<div class="projectContainer">
            <div class="thumbnail">
              <img src=${source} alt="project screenshot" />
            </div>
            <div class="about">
              <p class="title">${title}</p>
              <p class="description">
                ${description} 
              </p>
              <div class="links">
                <a href=${repo}
                  ><button class="github">Github</button></a
                >
                <a href=${preview}
                  ><button class="preview">Live</button></a
                >
              </div>
            </div>
          </div>`;
      // append to container
      const container = this.$refs.textContainer;
      container.appendChild(newProjectContainer);

      // scroll to bottom
      newProjectContainer.scrollIntoView();
      container.scrollTop = newProjectContainer.scrollHeight;
      // animate return animation promise
      const finishAnimation = await animate(
        `.text${this.nodeNumber}`,
        {
          opacity: 1,
          transform: ["scale(0.1)", "translateY(-10px) scale(1)"],
        },
        { endDelay: 1.5 },
        { duration: 0.4 },
        { easing: "ease-out" }
      ).finished;
      this.nodeNumber = this.nodeNumber + 1;
      return finishAnimation;
    },

    async yourWork() {
      this.$refs.controlArea.style.opacity = "0.7";

      let nodes = this.$refs.controlArea.getElementsByTagName("*");
      for (var i = 0; i < nodes.length; i++) {
        nodes[i].disabled = true;
      }

      await this.addNewTextMessage("Show me your best works", "senderMessage");
      const projects = [
        {
          title: "NLP project",
          source: "https://i.ibb.co/7KCmkJ8/ss-1.webp",
          description: `An application that identifies the tone of your posts, based on
                the interconnections between social platforms and publicly
                available data`,
          repo: "https://github.com/mayankjx/NLP-project",
          preview: "https://hackmanthan-nlp-project.herokuapp.com/",
        },
        {
          title: "GitHub Timeline Tracker",
          source:
            "https://raw.githubusercontent.com/mayankjx/github-timeline-tracker/main/project-ss.png",
          description:
            "Track public activities and repository activities of any user of gitHub",
          repo: "https://github.com/mayankjx/github-timeline-tracker",
          preview: "https://github-timeline-tracker.netlify.app/",
        },
        {
          title: "Planet Fact Site",
          source: "https://i.ibb.co/qnyYH8F/project-ss-1.webp",
          description:
            "8-page planets fact site with router history and dynamic navigation",
          repo: "https://github.com/mayankjx/planet-fact-site",
          preview: "https://planet-fact-site.netlify.app/",
        },
        {
          title: "Google Maps Clone",
          source: "https://i.ibb.co/gmrn7yG/Screenshot-2022-07-19-145313.webp",
          description:
            "Personalized college map for campus with responsive map controls and easy management via admin portal.",
          repo: "https://github.com/mayankjx/ClgMapGUI",
          preview: "#",
        },
        {
          title: "IP address tracker",
          source: "https://i.ibb.co/Dp8cCLk/project-ss.webp",
          description:
            "A single-page-application geo-locates a user based on client provided IP-Address, a user can also search for IP Address ",
          repo: "https://github.com/mayankjx/ip-address-tracker",
          preview: "https://ip-address-tracker-mayankj.netlify.app/",
        },
      ];

      for (let project of projects) {
        const animation = await this.renderProject(project);
      }

      this.$refs.controlArea.style.opacity = "1";
      for (var i = 0; i < nodes.length; i++) {
        nodes[i].disabled = false;
      }
    },

    async contact() {
      var controlArea = document.querySelector(".controlArea");
      controlArea.style.opacity = "0.5";
      var nodes = controlArea.getElementsByTagName("*");
      for (var i = 0; i < nodes.length; i++) {
        nodes[i].disabled = true;
      }

      await this.addNewTextMessage("How to contact you?", "senderMessage");
      const messages = [
        "You can mail me on prototype.mayank@gmail.com",
        "Connect with me on Linked in: https://www.linkedin.com/in/mayank-jhavre-5a713622a/",
        "Follow me on GitHub: https://github.com/mayankjx",
      ];

      for (let message of messages) {
        await this.addNewTextMessage(message, "message");
      }

      controlArea.style.opacity = "1";
      for (var i = 0; i < nodes.length; i++) {
        nodes[i].disabled = false;
      }
    },

    async resume() {
      this.$refs.controlArea.style.opacity = "0.7";

      let nodes = this.$refs.controlArea.getElementsByTagName("*");
      for (var i = 0; i < nodes.length; i++) {
        nodes[i].disabled = true;
      }

      const messages = ["Yes, here you go"];
      await this.addNewTextMessage("Can I see your resume?", "senderMessage");
      for (let message of messages) {
        const animation = await this.addNewTextMessage(message, "message");
      }
      const newText = document.createElement("div");
      const container = this.$refs.textContainer;

      newText.classList.add("message");
      newText.classList.add(`text${this.nodeNumber}`);

      newText.innerHTML = `<div class="resumeBlock">
      <a href="https://mj-resume.tiiny.site/"><img src = "./assets/pdf.png"}/"><p> resume.pdf</p></a>
      </div>`;

      container.appendChild(newText);

      newText.scrollIntoView();
      container.scrollTop = newText.scrollHeight;

      this.nodeNumber = this.nodeNumber + 1;

      this.$refs.controlArea.style.opacity = "1";
      for (var i = 0; i < nodes.length; i++) {
        nodes[i].disabled = false;
      }
    },

    sendText() {
      this.addNewTextMessage(this.message, "senderMessage");
      this.message = "";
    },
  },
  mounted() {
    let textMessages = document.getElementsByClassName("message");
    for (const message of textMessages) {
      message.style.opacity = "0";
      message.style.position = "relative";
      message.style.left = "-10px";
    }
    const text1 = [
      ".text1",
      {
        opacity: 1,
        transform: [
          "scale(0.1)",
          "translateX(10px) scale(1.2)",
          "translateX(10px) scale(1)",
        ],
        width: ["0", "11rem"],
      },
      { duration: 1 },
      { easing: "ease-in" },
    ];
    const text2 = [
      ".text2",
      {
        opacity: 1,
        transform: [
          "scale(0.1)",
          "translateX(10px) scale(1.2)",
          "translateX(10px) scale(1)",
        ],
        width: ["0", "10rem"],
      },
      { duration: 1.5 },
      { easing: "ease-in" },
    ];
    const text3 = [
      ".text3",
      {
        opacity: 1,
        transform: [
          "scale(0.1)",
          "translateX(10px) scale(1.2)",
          "translateX(10px) scale(1)",
        ],
        width: ["0", "25rem"],
      },
      { duration: 2 },
      { easing: "ease-in" },
    ];
    const text4 = [
      ".text4",
      {
        opacity: 1,
        transform: [
          "scale(0.1)",
          "translateX(10px) scale(1.2)",
          "translateX(10px) scale(1)",
        ],
        width: ["0", "27rem"],
      },
      { duration: 2 },
      { easing: "ease-in" },
    ];
    const text5 = [
      ".text5",
      {
        opacity: 1,
        transform: [
          "scale(0.1)",
          "translateX(10px) scale(1.2)",
          "translateX(10px) scale(1)",
        ],
        width: ["0", "30rem"],
      },
      { duration: 2 },
      { easing: "ease-in" },
    ];
    const text6 = [
      ".text6",
      {
        opacity: 1,
        transform: [
          "scale(0.1)",
          "translateX(10px) scale(1.2)",
          "translateX(10px) scale(1)",
        ],
        width: ["0", "16rem"],
      },
      { duration: 2 },
      { easing: "ease-in" },
    ];
    const text7 = [
      ".text7",
      {
        opacity: 1,
        transform: [
          "scale(0.1)",
          "translateX(-10px) scale(1.2)",
          "translateX(-10px) scale(1)",
        ],
        width: ["0", "7rem"],
      },
      { duration: 1 },
      { easing: "ease-in" },
    ];
    const sequence = [text1, text2, text3, text4, text5, text6, text7];
    const timelineAnimations = timeline(sequence, { endDelay: 0.3 });
  },
};
</script>

<style lang="scss">
@import "./assets/Sass/layout/text-area";
@import "./assets/Sass/components/send-controls";
@import "./assets/Sass/components/resume-block";
@import "./assets/Sass/components/projectContainer";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  // overflow: auto;
  width: 100vw;
  height: 100vh;
  padding: 1rem 1.4rem;
  font-size: 1.5rem;
}

.wrapper {
  height: 100%;
}

@media screen and (max-width: 768px) {
  #app {
    font-size: 1.2rem;
  }
}

@media screen and (max-width: 1200px) and (min-width: 768px) {
  #app {
    font-size: 1.4rem;
  }
}
</style>
