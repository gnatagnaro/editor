<template>
  <div class="content">
    <div :class="['listContainer', { active: isMenuOpen }]">
      <button class="moreButton" aria-label="Menu Button" @click="toggleListContainer">
        <div class="menuIconWrapper">
          <div class="menuIconLine half first"></div>
          <div class="menuIconLine"></div>
          <div class="menuIconLine half last"></div>
        </div>
      </button>
      <ul class="moreButtonList">

        <li class="moreButtonListItem">
          <button @click="$emit('show')" class="uploadButton"><svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-share">
            <path d="M4 12v8a2 2 0 002 2h12a2 2 0 002-2v-8M16 6l-4-4-4 4M12 2v13"/>
          </svg>
          </button>
        </li>

        <li class="moreButtonListItem">
          <button @click="$emit('pippet')" class="uploadButton"><svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-share"><path d="M2 22l2-2h3l7-7" /><path d="M4 20v-3l7-7" /><path d="M14.29 13.3a1 1 0 001.41 0l.8-.8c.27-.27.27-.72 0-1s-.28-.72 0-1l4.08-4.08a2 2 0 000-2.83l-.17-.17a2 2 0 00-2.83 0L13.5 7.51c-.28.27-.73.27-1 0s-.73-.28-1 0l-.8.79a1 1 0 000 1.41l3.59 3.59z" /></svg>
          </button>
        </li>

        <li class="moreButtonListItem">
          <button @click="$emit('grab')" class="uploadButton"><svg width="25" height="25" viewBox="0 0 21 21" xmlns="http://www.w3.org/2000/svg"><g fill="none" fill-rule="evenodd" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" transform="translate(5 6)"><path d="m2.35073766 3.50548574.14926234-.00548574v-1c0-.55228475.44771525-1 1-1s1 .44771525 1 1v-1c0-.55228475.44771525-1 1-1s1 .44771525 1 1v1c0-.55228475.44771525-1 1-1s1 .44771525 1 1v1c0-.55228475.44771525-1 1-1 .5522847 0 1 .44771525 1 1v4c0 2.7614237-2.23857625 5-5 5h-.5c-2.48528137 0-4.5-2.0147186-4.5-4.5v-2.5c0-1.0543618.81587779-1.91816512 1.85073766-1.99451426z"/><path d="m2.5 2.5v3"/><path d="m4.5 1.5v2"/><path d="m6.5 1.5v2"/><path d="m8.5 2.5v2"/></g></svg>
          </button>
        </li>

        <li class="moreButtonListItem">
          <button @click="$emit('scale')" class="uploadButton"><svg width="20" height="20" viewBox="0 0 96 96" stroke="currentColor" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M12 72V24H84V72H12ZM18 30V66H78V30H18ZM78 12H96V30H90V18H78V12ZM6 18V30H0V12H18V18H6ZM90 78V66H96V84H78V78H90ZM6 66V78H18V84H0V66H6Z" fill="currentColor"/></svg>
          </button>
        </li>

        <li class="moreButtonListItem">
          <button @click="$emit('curve')" class="uploadButton"><svg stroke="currentColor" width="20" height="20" viewBox="0 0 32 32" version="1.1" xmlns="http://www.w3.org/2000/svg"><path d="M29.5 7c-1.381 0-2.5 1.12-2.5 2.5 0 0.284 0.058 0.551 0.144 0.805l-6.094 5.247c-0.427-0.341-0.961-0.553-1.55-0.553-0.68 0-1.294 0.273-1.744 0.713l-4.774-2.39c-0.093-1.296-1.162-2.323-2.482-2.323-1.38 0-2.5 1.12-2.5 2.5 0 0.378 0.090 0.732 0.24 1.053l-4.867 5.612c-0.273-0.102-0.564-0.166-0.873-0.166-1.381 0-2.5 1.119-2.5 2.5s1.119 2.5 2.5 2.5c1.381 0 2.5-1.119 2.5-2.5 0-0.332-0.068-0.649-0.186-0.939l4.946-5.685c0.236 0.073 0.48 0.124 0.74 0.124 0.727 0 1.377-0.316 1.834-0.813l4.669 2.341c0.017 1.367 1.127 2.471 2.497 2.471 1.381 0 2.5-1.119 2.5-2.5 0-0.044-0.011-0.086-0.013-0.13l6.503-5.587c0.309 0.137 0.649 0.216 1.010 0.216 1.381 0 2.5-1.119 2.5-2.5s-1.119-2.5-2.5-2.5z"></path></svg>
          </button>
        </li>

        <li class="moreButtonListItem">
          <button @click="$emit('filtering')" class="uploadButton"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M3 4.6C3 4.03995 3 3.75992 3.10899 3.54601C3.20487 3.35785 3.35785 3.20487 3.54601 3.10899C3.75992 3 4.03995 3 4.6 3H19.4C19.9601 3 20.2401 3 20.454 3.10899C20.6422 3.20487 20.7951 3.35785 20.891 3.54601C21 3.75992 21 4.03995 21 4.6V6.33726C21 6.58185 21 6.70414 20.9724 6.81923C20.9479 6.92127 20.9075 7.01881 20.8526 7.10828C20.7908 7.2092 20.7043 7.29568 20.5314 7.46863L14.4686 13.5314C14.2957 13.7043 14.2092 13.7908 14.1474 13.8917C14.0925 13.9812 14.0521 14.0787 14.0276 14.1808C14 14.2959 14 14.4182 14 14.6627V17L10 21V14.6627C10 14.4182 10 14.2959 9.97237 14.1808C9.94787 14.0787 9.90747 13.9812 9.85264 13.8917C9.7908 13.7908 9.70432 13.7043 9.53137 13.5314L3.46863 7.46863C3.29568 7.29568 3.2092 7.2092 3.14736 7.10828C3.09253 7.01881 3.05213 6.92127 3.02763 6.81923C3 6.70414 3 6.58185 3 6.33726V4.6Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>
          </button>
        </li>

        <li class="moreButtonListItem">
          <button @click="$emit('save')" class="uploadButton"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M15 20V15H9V20M18 20H6C4.89543 20 4 19.1046 4 18V6C4 4.89543 4.89543 4 6 4H14.1716C14.702 4 15.2107 4.21071 15.5858 4.58579L19.4142 8.41421C19.7893 8.78929 20 9.29799 20 9.82843V18C20 19.1046 19.1046 20 18 20Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>
          </button>
        </li>

        <li class="moreButtonListItem">
          <button @click="$emit('clear')" class="uploadButton"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" clip-rule="evenodd" d="M8.53113 1C5.52364 1 3.19671 3.63591 3.56974 6.62017L5.28873 20.3721C5.47639 21.8734 6.7526 23 8.26557 23H15.7344C17.2474 23 18.5236 21.8734 18.7113 20.3721L20.4303 6.62017C20.8033 3.63591 18.4764 1 15.4689 1H8.53113ZM5.70148 5C6.11066 3.8455 7.21175 3 8.53113 3H15.4689C16.7883 3 17.8893 3.8455 18.2985 5H5.70148ZM5.63279 7L7.27329 20.124C7.33584 20.6245 7.76124 21 8.26557 21H15.7344C16.2388 21 16.6642 20.6245 16.7267 20.124L18.3672 7H5.63279Z" fill="currentColor"/><path d="M15.002 10.998C14.6114 10.6075 13.9783 10.6075 13.5878 10.998L12 12.5858L10.4201 11.0058C10.0296 10.6153 9.3964 10.6153 9.00587 11.0058C8.61535 11.3964 8.61535 12.0295 9.00587 12.4201L10.5858 14L9.00001 15.5858C8.60949 15.9763 8.60949 16.6095 9.00001 17C9.39054 17.3905 10.0237 17.3905 10.4142 17L12 15.4142L13.5878 17.0019C13.9783 17.3925 14.6114 17.3925 15.002 17.0019C15.3925 16.6114 15.3925 15.9782 15.002 15.5877L13.4142 14L15.002 12.4123C15.3925 12.0217 15.3925 11.3886 15.002 10.998Z" fill="currentColor"/></svg>
          </button>
        </li>

      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "MenuPanel",
  props: {
    state: String,
    startImage: Object,
  },
  data() {
    return {
      new: null,
      isMenuOpen: false,
    };
  },
  methods: {
    toggleListContainer() {
      this.isMenuOpen = !this.isMenuOpen;
    },
  },
  computed: {
    isPippet() {
      return this.state === 'pippet';
    },
    isGrab() {
      return this.state === 'grab';
    },
    isCurves() {
      return this.state === 'curves';
    },
    isFiltering() {
      return this.state === 'filter';
    }
  }
};
</script>

<style scoped>
.listContainer {
  z-index: 9999;
  position: fixed;
  bottom: 5%;
  right: 1%;
}

.listContainer.active .moreButtonList {
  opacity: 1;
  transform: scale(1);
}

.listContainer.active .moreButtonListItem {
  animation: fadeInItem 0.8s 0.2s forwards;
}

.listContainer.active .moreButtonListItem:nth-child(2) {
  animation-delay: 0.2s;
}

.listContainer.active .moreButtonListItem:nth-child(3) {
  animation-delay: 0.3s;
}

.listContainer.active .moreButtonListItem:nth-child(4) {
  animation-delay: 0.4s;
}

.listContainer.active .moreButtonListItem:nth-child(5) {
  animation-delay: 0.5s;
}

.listContainer.active .moreButtonListItem:nth-child(6) {
  animation-delay: 0.6s;
}

.listContainer.active .moreButtonListItem:nth-child(7) {
  animation-delay: 0.7s;
}

.listContainer.active .moreButtonListItem:nth-child(8) {
  animation-delay: 0.8s;
}

.listContainer.active .moreButton {
  animation: onePulse 0.8s forwards linear;
}

.listContainer.active .menuIconWrapper {
  transform: rotate(-45deg);
}

.listContainer.active .menuIconLine.first {
  transform: rotate(-90deg) translateX(1px);
}

.listContainer.active .menuIconLine.last {
  transform: rotate(-90deg) translateX(-1px);
}

.moreButton {
  background-color: gray;
  box-shadow: 0px 0px 0px 4px rgba(92, 103, 255, 0.3);
  border-radius: 50%;
  width: 50px;
  height: 50px;
  border: none;
  padding: 0;
  cursor: pointer;
  transition: 0.2s ease-in;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #fff;
  position: relative;
  z-index: 2;
}

.moreButton:hover,
.moreButton:focus {
  box-shadow: 0px 0px 0px 8px rgba(92, 103, 255, 0.3);
  background-color: gray;
}

.moreButton:focus {
  outline: 0;
}

.moreButtonList {
  background-color: #fff;
  border-radius: 8px;
  list-style-type: none;
  width: 60px;
  height: 400px;
  box-shadow: 0px 0px 4px 4px rgba(150, 157, 249, 0.16);
  padding: 0;
  padding: 6px;
  position: absolute;
  right: -5px;
  bottom: 60px;
  opacity: 0;
  transform: scale(0);
  transform-origin: bottom center;
  transition: all 0.3s ease 0.1s;
}

.moreButtonList li {
  opacity: 0;
}

.moreButtonListItem {
  display: flex;
  align-items: center;
  color: gray;
  padding: 10px;
  border-radius: 4px;
  cursor: pointer;
  position: relative;
  transition: 0.2s ease-in;
  transform: translateX(-10px);
}

.moreButtonListItem:hover {
  color: gray;
}

.moreButtonListItem:after {
  content: '';
  position: absolute;
  height: 1px;
  width: calc(100% - 24px);
  left: 12px;
  bottom: 0;
  background-color: rgba(132, 160, 244, 0.1);
}

.moreButtonListItem:last-child:after {
  display: none;
}

.moreButtonList-item svg {
  width: 18px;
  height: 18px;
}

.moreButtonListItem button {
  background: none;
  border: none;
  cursor: pointer;
  color: inherit;
  font: inherit;
  padding: 0;
  margin: 0 5px;
  outline: none;
}

@keyframes fadeInItem {
  100% {
      transform: translateX(0px);
      opacity: 1;
  }
}

@keyframes onePulse {
  0% {
      box-shadow: 0px 0px 0px 0px rgba(92, 103, 255, 0.3);
  }

  50% {
      box-shadow: 0px 0px 0px 12px rgba(92, 103, 255, 0.1);
  }

  100% {
      box-shadow: 0px 0px 0px 4px rgba(92, 103, 255, 0.3);
  }
}

.menuIconWrapper {
  border-radius: 2px;
  width: 20px;
  height: 20px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  cursor: pointer;
  transition: transform 330ms ease-out;
}

.menuIconLine {
  background-color: #fff;
  border-radius: 2px;
  width: 100%;
  height: 2px;
}

.menuIconLine.half {
  width: 50%;
}

.menuIconLine.first {
  transition: transform 300ms cubic-bezier(0.52, -0.80, 0.52, 0.52);
  transform-origin: right;
}

.menuIconLine.last {
  align-self: flex-end;
  transition: transform 300ms cubic-bezier(0.52, -0.80, 0.52, 0.52);
  transform-origin: left;
}
</style>