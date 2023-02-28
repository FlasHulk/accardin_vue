<template>
  <div class="container">
    <div class="accordeon">
      <div class="accordeon__item"
           v-for="(item, key) in accordeon"
           :key="key"
      >
        <div
            class="accordeon__item--title"
            :class="item.open ? 'active' : ''"
            @click="item.open = !item.open"
        >
          {{ item.title }}

          <svg xmlns="http://www.w3.org/2000/svg" width="12" height="8" viewBox="0 0 12 8" fill="none">
            <path d="M1 1L6 6L11 1" stroke="#0D2A54" stroke-width="2"/>
          </svg>
        </div>

        <transition
            name="expand"
            @enter="enter"
            @after-enter="afterEnter"
            @leave="leave"
        >
          <div class="accordeon__item-wrap"  v-show="item.open">
            <div
                class="accordeon__item--info"
                v-for="(option, key) in item.options"
                :key="key"
            >

              <div class="info-title"
                   :class="option.open ? 'active' : ''"
                   @click="option.open = !option.open"
              >
                {{ option.option_title }}

                <svg xmlns="http://www.w3.org/2000/svg" width="12" height="8" viewBox="0 0 12 8" fill="none">
                  <path d="M1 1L6 6L11 1" stroke="#2B41B7" stroke-width="2"/>
                </svg>
              </div>

              <transition
                  name="expand"
                  @enter="enter"
                  @after-enter="afterEnter"
                  @leave="leave"
              >
              <div class="info-options-wrap" v-show="option.open">
                <div
                    class="info-options"
                    v-for="(item, key) in option.option_lists"
                    :key="key"
                >
                  <label class="info-options-radio">
                    <input type="radio" :name="item.category" :value="item.value">
                    <span class="radio"></span>
                    <p>
                      {{ item.name }}
                    </p>
                  </label>
                </div>
              </div>
              </transition>
            </div>
          </div>
        </transition>

      </div>

    </div>
  </div>
</template>

<script>
export default {
  name: "HomePage",
  inject: ["Accordion"],
  data() {
    return {
      accordeon: [
        {
          title: 'The analysis',
          options: [
            {
              option_title: 'To take into account on user level data',
              option_lists: [
                {
                  category: 't1',
                  name: 'Do you have one user in more than one variant of the same AB test?',
                  val: 1
                },
                {
                  category: 't1',
                  name: 'Are you users actually getting the treatment we expect them to get?',
                  val: 2
                },
                {
                  category: 't1',
                  name: 'Do we have enough users in our AB test? Are those users representative for the total population?',
                  val: 3
                }
              ],
              open: false
            }
          ],
          open: false
        },
        {
          title: 'The analysis 2',
          options: [
            {
              option_title: 'To take into account on user level data2',
              option_lists: [
                {
                  category: 't2',
                  name: 'Do you have one user in more than one variant of the same AB test? 2',
                  val: 1
                },
                {
                  category: 't2',
                  name: 'Are you users actually getting the treatment we expect them to get? 2',
                  val: 2
                },
                {
                  category: 't2',
                  name: 'Do we have enough users in our AB test? Are those users representative for the total population? 2',
                  val: 3
                }
              ],
              open: false
            }
          ],
          open: false
        },
        {
          title: 'The analysis 3',
          options: [
            {
              option_title: 'To take into account on user level data 3',
              option_lists: [
                {
                  category: 't3',
                  name: 'Do you have one user in more than one variant of the same AB test? 3',
                  val: 1
                },
                {
                  category: 't3',
                  name: 'Are you users actually getting the treatment we expect them to get? 3',
                  val: 2
                },
                {
                  category: 't3',
                  name: 'Do we have enough users in our AB test? Are those users representative for the total population? 3',
                  val: 3
                }
              ],
              open: false
            }
          ],
          open: false
        }
      ],

    }
  },
  methods: {
    enter(element) {
      const width = getComputedStyle(element).width;

      element.style.width = width;
      element.style.position = 'absolute';
      element.style.visibility = 'hidden';
      element.style.height = 'auto';

      const height = getComputedStyle(element).height;
      element.style.width = null;
      element.style.position = null;
      element.style.visibility = null;
      element.style.height = 0;

      getComputedStyle(element).height;

      requestAnimationFrame(() => {
        element.style.height = height;
      });
    },
    afterEnter(element) {
      element.style.height = 'auto';
    },
    leave(element) {
      const height = getComputedStyle(element).height;

      element.style.height = height;

      getComputedStyle(element).height;

      requestAnimationFrame(() => {
        element.style.height = 0;
      });
    },
  },

}
</script>

<style lang="scss" scoped>
.accordeon {
  background: #FFFFFF;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.02), 0px 4px 17px rgba(0, 0, 0, 0.14), 0px 4px 22px rgba(0, 0, 0, 0.12);
  border-radius: 8px;

  .accordeon__item {
    .accordeon__item--title {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 16px 23px 16px 16px;
      font-weight: 800;
      font-size: 16px;
      line-height: 24px;
      color: #0D2A54;
      border-bottom: 1px solid #D0DBF1;
      cursor: pointer;


      &.active {
        svg {
          transform: rotate(180deg);
        }
      }

      svg {
        display: block;
        margin-left: 10px;
        min-width: 10px;
        width: 10px;
        transition: .3s;
      }
    }

    .accordeon__item:last-child {
      .accordeon__item--title {
        border-bottom: unset;
      }
    }

    .accordeon__item--info {
      background: #FAF8F6;
      padding: 16px;

      .info-title {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding-right: 7px;
        margin-bottom: 8px;
        font-weight: 800;
        font-size: 16px;
        line-height: 24px;
        color: #2B41B7;
        cursor: pointer;

        &.active {
          svg {
            transform: rotate(180deg);
          }
        }

        svg {
          display: block;
          min-width: 10px;
          margin-left: 10px;
          transition: .3s;
        }
      }

      .info-options {
        .info-options-radio {
          display: flex;

          & + .info-options-radio {
            padding-top: 8px;
          }

          input {
            position: absolute;
            opacity: 0;
            z-index: -1;
          }

          input:checked + .radio {
            background: #2B41B7;
          }

          .radio {
            display: block;
            min-width: 12px;
            width: 12px;
            height: 12px;
            margin-top: 6px;
            margin-right: 8px;
            border: 2px solid #2B41B7;
            border-radius: 50%;
          }

          p {
            font-weight: 600;
            font-size: 14px;
            line-height: 24px;
            color: #0D2A54;
          }
        }
      }
    }
  }
}

.expand-enter-active,
.expand-leave-active {
  transition: height 300ms ease-in-out;
  overflow: hidden;
}

.expand-enter,
.expand-leave-to {
  height: 0;
}
</style>