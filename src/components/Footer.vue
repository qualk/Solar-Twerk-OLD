<template>
  <div id="footer-container">
    <div id="solar-twerk-container">
      <img
        src="@/assets/logo-gray.svg"
        id="footer-image"
        alt="Solar Twerk Logo"
      />
      <h4 id="solartwerk-text" class="footer-text">
        Solar Twerk • {{ version }} • {{ os }} •
        {{ served ? "Development (Served)" : "Production" }}
      </h4>
    </div>
    <div id="links-container">
      <ul id="links">
        <li
          v-for="link in links"
          v-bind:key="link.url"
          class="link"
          @click="openLink(link.url)"
        >
          <i :class="link.icon" style="color: var(--color-footer-text)"></i>
        </li>
      </ul>
    </div>
    <div id="mojang-disclaimer-container">
      <h4 class="footer-text">Not affiliated with Mojang, AB.</h4>
    </div>
  </div>
</template>

<script>
import { remote } from "electron";
import { platform } from "process";
import constants from "../constants";

export default {
  name: "Footer",

  data: () => ({
    version: remote.app.getVersion(),
    os:
      platform === "win32"
        ? "Windows"
        : platform === "darwin"
        ? "MacOS"
        : platform === "linux"
        ? "Linux"
        : `Unknown (${platform})`,
    links: [
      {
        icon: "fa-brands fa-github",
        url: constants.links.GITHUB,
      },
      {
        icon: "fa-solid fa-comments",
        url: constants.links.GH_DISCUSSIONS,
      },
      {
        icon: "fa-brands fa-youtube",
        url: constants.links.YOUTUBE,
      },
      {
        icon: "fa-solid fa-globe",
        url: constants.links.WEBSITE,
      },
    ],
    served: !remote.app.isPackaged,
  }),

  methods: {
    /**
     * Opens a link in the default browser
     * @param {string} url The url to open
     */
    openLink(url) {
      remote.shell.openExternal(url);
    },
  },
};
</script>

<style scoped>
#footer-container {
  position: fixed;
  background-color: var(--card-background);
  bottom: 0;
  height: 50px;
  width: 100%;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  z-index: 10;
}

#solar-twerk-container {
  display: flex;
}

#footer-image {
  opacity: 0.3;
  margin-left: 15px;
  height: 30px;
  -webkit-user-drag: none;
}

.footer-text {
  color: var(--color-footer-text);
  font-weight: 600;
  letter-spacing: 0.25px;
  font-size: 17px;
}

#client-text {
  margin-left: 10px;
  margin-top: auto;
  margin-bottom: auto;
}

#links {
  position: absolute;
  left: 50%;
  bottom: 12.5px;
  transform: translateX(-50%);
  list-style-type: none;
}

.link {
  display: inline;
  margin: 0 10px;
  color: var(--color-footer-text);
  font-size: 20px;
  cursor: pointer;
  transition: 0.2s ease;
}

.link:hover {
  filter: brightness(1.5);
}

#mojang-disclaimer-container {
  font-weight: 200;
  letter-spacing: 0.2px;
  justify-self: right;
  margin-right: 15px;
  margin-left: 25px;
}
</style>
