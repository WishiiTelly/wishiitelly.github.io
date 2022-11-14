<!-- saved from url=(0053)file:///C:/Users/user/Desktop/resources/website.html -->
<html>

<head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">

  <title>AeroGlass Website</title>
  <script>
    var uniqueId = 0;

    function genUniqueKeyframeName() {
      return "seq-" + uniqueId++
    }
    var uniqueId = 0,
      SceneItem = function (n, t, i, r, u) {
        n.style.visibility = "visible";
        this.keyframe_sequence = u;
        this.element = n;
        this.animation_name = t;
        this.animation_duration = r;
        this.keyframes = "@-ms-keyframes " + t + " {" + i + "}"
      },
      KeyframeSequencer = function (n) {
        this._created_keyframes = [];
        this.container_element = n;
        this._sequences = [];
        var t = document.createElement("style");
        document.head.appendChild(t);
        this.stylesheet = t.sheet
      },
      ScenesManager;
    KeyframeSequencer.prototype.addSequence = function (n, t, i, r) {
      var u = {},
        e, s, f, o, h, c;
      for (u.element = n, u.keyframe_name = genUniqueKeyframeName(), u.keyframe_style_str = "@-ms-keyframes " + u
        .keyframe_name + " {", e = 0, f = 0; f < t.length; f++) o = t[f].duration, e += o;
      for (s = 0, f = 0; f < t.length; f++) o = t[f].duration, s += o, h = s / e * 100 + "%", c = t[f].properties, u
        .keyframe_style_str += h + "{" + c + "}";
      u.keyframe_style_str += "}";
      u.delay = i;
      u.duration = e + "s";
      u.fill_mode = r || "both";
      this._sequences.push(u)
    };
    KeyframeSequencer.prototype.playSequence = function () {
      for (var n, i, r = [], t = 0; t < this.container_element.childNodes.length; t++) r.push(this.container_element
        .childNodes[t]);
      for (this.stylesheet.cssText = "", t = 0; t < this._sequences.length; t++) n = this._sequences[t], i = n
        .element, this.stylesheet.insertRule(n.keyframe_style_str, 0), this._created_keyframes.push(n.keyframe_name),
        i.style.msAnimationDuration = n.duration, i.style.msAnimationDelay = n.delay, i.style.msAnimationFillMode = n
        .fill_mode, i.style.msAnimationName = n.keyframe_name
    };
    ScenesManager = function (n) {
      this._event_element = document.createElement("div");
      this._canvas = n;
      this._scenes = [];
      this._playhead = 0;
      this._repeat = !1;
      this._repeat_from = 0;
      this._iteration_count = 0;
      this._max_iteration = 0;
      this.oncomplete_callback = null;
      var t = document.createElement("style");
      document.head.appendChild(t);
      this.stylesheet = t.sheet
    };
    ScenesManager.prototype.addScene = function (n) {
      this._scenes.push(n)
    };
    ScenesManager.prototype.playScenes = function (n) {
      while (this._canvas.hasChildNodes()) this._canvas.removeChild(this._canvas.childNodes[0]);
      n && n.repeat && (this._repeat = n.repeat, n && n.repeat_from && (this._repeat_from = n.repeat_from), n && n
        .repeat_count && (this._max_iteration = n.repeat_count));
      n && n.oncomplete_callback && (this.oncomplete_callback = n.oncomplete_callback);
      this.playCurrentScene()
    };
    ScenesManager.prototype._onPlayNext = function () {
      var n = !1;
      this._playhead + 1 < this._scenes.length ? (this._playhead += 1, this.playCurrentScene()) : (this.stylesheet
        .cssText = "", this._repeat ? (this._playhead = this._repeat_from, this._max_iteration != 0 && this
          ._iteration_count >= this._max_iteration - 1 ? (this.repeat = !1, n = !0) : (this.playCurrentScene(), this
            ._iteration_count += 1)) : n = !0);
      n && this.oncomplete_callback != null && this.oncomplete_callback()
    };
    ScenesManager.prototype.playCurrentScene = function () {
      var u = this,
        t = this._scenes[this._playhead],
        n = t.element,
        i, r;
      this._canvas.appendChild(n);
      i = t.keyframes;
      r = this.stylesheet;
      r.insertRule(i, 0);
      n.has_event || (n.has_event = !0, n.addEventListener("MSAnimationEnd", function (t) {
        t.target === n && u._onPlayNext()
      }));
      n.style.msAnimationName = t.animation_name;
      n.style.msAnimationDuration = t.animation_duration;
      n.style.msAnimationFillMode = "both";
      t.keyframe_sequence && t.keyframe_sequence.playSequence()
    }
    var rtl_mode = false,
      v_offset = 40;

    function setVerticalOffset(n) {
      v_offset = n
    }

    function setTextStyle(n, t) {
      _setTextStyle(n, t, ".instruction_text, .final_text")
    }

    function setSubTextStyle(n, t) {
      _setTextStyle(n, t, ".instruction_text_small, .final_text_small")
    }

    function _setTextStyle(n, t, i) {
      for (var r, u = n.split(";"), o = u[0] + "pt", s = u[1], h = u[2], e = document.querySelectorAll(i), f = 0; f < e
        .length; f++) r = e[f], r.style.fontSize = o, r.style.fontFamily = s, r.style.fontWeight = h, r.style
        .fontColor = t
    }

    function setRTL(n) {
      rtl_mode = n;
      var t;
      n ? (t = document.getElementById("playback_canvas"), t.parentElement.removeChild(t)) : (t = document
        .getElementById("playback_canvas_rtl"), t.parentElement.removeChild(t))
    }

    function initTryThisLaterMessageSequence(n, t, i) {
      var u = document.getElementById("trythislater_overlay_canvas"),
        r = new ScenesManager(u);
      r.addScene(new SceneItem(document.getElementById("trythislater_sequence"), "s0", "to{opacity:0}", "0s", null));
      r.addScene(new SceneItem(document.getElementById("trythislater_sequence"), "s0",
        "from{-ms-animation-timing-function:ease;opacity:0;}15%{opacity:1}85%{opacity:1}to{opacity:1}", n, null));
      r.addScene(new SceneItem(document.getElementById("trythislater_sequence"), "s0", "to{opacity:1}", t, null));
      r.addScene(new SceneItem(document.getElementById("trythislater_sequence"), "s0",
        "from{-ms-animation-timing-function:ease;opacity:1;}15%{opacity:1}85%{opacity:1}to{opacity:0}", i, null));
      r.playScenes({
        repeat: !1
      })
    }

    function initEndSequence() {
      document.getElementById("end_sequence_canvas").style.visibility = "visible";
      var n, t = document.getElementById("end_sequence_canvas");
      n = new ScenesManager(t);
      n.addScene(new SceneItem(document.getElementById("pre_end_sequence"), "es", "to{opacity:1}", "6.2s",
        setPreEndSequence()));
      n.addScene(new SceneItem(document.getElementById("end_sequence"), "es", "to{opacity:1}", "0s", setEndSequence()));
      n.playScenes({
        repeat: !1,
        oncomplete_callback: function () {
          startAnimation()
        }
      })
    }

    function initEndSequenceMessagesOverlay() {
      document.getElementById("final_messages_overlay_canvas").style.visibility = "visible";
      var n, t = document.getElementById("final_messages_overlay_canvas");
      n = new ScenesManager(t);
      n.addScene(new SceneItem(document.getElementById("message_overlay_sequence"), "mos", "to{opacity:1}", "300s",
        setFinalMessagesSequence()));
      n.playScenes({
        repeat: !1
      })
    }

    function addZDPMessage(n) {
      rgZDPMessages[rgZDPMessages.length] = n
    }

    function initZDPEndSequence() {
      initEndSequence();
      document.getElementById("zdp_final_messages_overlay_canvas").style.visibility = "visible";
      nextZDPMessage()
    }

    function nextZDPMessage() {
      document.getElementById("zdp_final_message_text").innerHTML = rgZDPMessages[zdpMessageIndex++];
      var n, t = document.getElementById("zdp_final_messages_overlay_canvas");
      n = new ScenesManager(t);
      n.addScene(new SceneItem(document.getElementById("zdp_message_overlay_sequence"), "mos", "to{opacity:1}", "60s",
        setZDPFinalMessagesSequence()));
      n.playScenes({
        repeat: !1,
        oncomplete_callback: function () {
          zdpMessageIndex >= rgZDPMessages.length && (zdpMessageIndex = 0);
          nextZDPMessage()
        }
      })
    }

    function setFinalMessagesSequence() {
      var i = new KeyframeSequencer(document.getElementById("message_overlay_sequence"));
      var r = 60,
        n = r - 7,
        t = n;
      return i.addSequence(document.getElementById("final_message_text1"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 6
      }, {
        properties: "-ms-animation-timing-function:linear;-ms-transform:translate(0px,0px);opacity:1;",
        duration: n
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0;",
        duration: 1
      }], "0s"), i.addSequence(document.getElementById("sub_final_message_text1"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 6
      }, {
        properties: "-ms-animation-timing-function:linear;-ms-transform:translate(0px,0px);opacity:1;",
        duration: n + 2 + (r - 2)
      }, {
        properties: "-ms-animation-timing-function:linear;-ms-transform:translate(0px,0px);opacity:0;",
        duration: 1
      }], "0s"), t += 7, n = r - 2, i.addSequence(document.getElementById("final_message_text2"), [{
        properties: "-ms-animation-timing-function:linear;opacity:0;-ms-transform:translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: n
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0;",
        duration: 1
      }], t + "s"), i.addSequence(document.getElementById("sub_final_message_text2"), [{
        properties: "-ms-animation-timing-function:linear;opacity:0;-ms-transform:translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0;",
        duration: 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0;",
        duration: n
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0;",
        duration: 1
      }], t + "s"), t += n + 2, i.addSequence(document.getElementById("final_message_text3"), [{
        properties: "-ms-animation-timing-function:linear;opacity:0;-ms-transform:translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: n
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0;",
        duration: 1
      }], t + "s"), i.addSequence(document.getElementById("sub_final_message_text3"), [{
        properties: "-ms-animation-timing-function:linear;opacity:0;-ms-transform:translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: n * 3 + 4
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 1
      }], t + "s"), t += n + 2, i.addSequence(document.getElementById("final_message_text4"), [{
        properties: "-ms-animation-timing-function:linear;opacity:0;-ms-transform:translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: n
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0;",
        duration: 1
      }], t + "s"), i.addSequence(document.getElementById("sub_final_message_text4"), [{
        properties: "-ms-animation-timing-function:linear;opacity:0;-ms-transform:translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0;",
        duration: 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: n + 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0;",
        duration: 1
      }], t + (n + 2) * 2 + "s"), t += n + 2, i.addSequence(document.getElementById("final_message_text5"), [{
        properties: "-ms-animation-timing-function:linear;opacity:0;-ms-transform:translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: n
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 1
      }], t + "s"), i.addSequence(document.getElementById("sub_final_message_text5"), [{
        properties: "-ms-animation-timing-function:linear;opacity:0;-ms-transform:translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0;",
        duration: 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0;",
        duration: n + 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0;",
        duration: 1
      }], t + (n + 2) * 3 + "s"), i
    }

    function setZDPFinalMessagesSequence() {
      var n, t, i;
      return n = new KeyframeSequencer(document.getElementById("zdp_message_overlay_sequence")), t = 60, i = t - 7, n
        .addSequence(document.getElementById("zdp_final_message_text"), [{
          properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
          duration: 0
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:1;",
          duration: 6
        }, {
          properties: "-ms-animation-timing-function:linear;-ms-transform:translate(0px,0px);opacity:1;",
          duration: i
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }], "0s"), n
    }

    function startAnimation() {
      elm = document.getElementById("color1");
      rCurrent = rValues[currentColor];
      gCurrent = gValues[currentColor];
      bCurrent = bValues[currentColor];
      setTargetColor()
    }

    function setTargetColor() {
      rIncrement = (rValues[targetColor] - rCurrent) / steps;
      gIncrement = (gValues[targetColor] - gCurrent) / steps;
      bIncrement = (bValues[targetColor] - bCurrent) / steps;
      currentColor = (currentColor + 1) % numberColors;
      targetColor = (targetColor + 1) % numberColors;
      currentStep = steps;
      cycleToTargetColor()
    }

    function cycleToTargetColor() {
      currentStep--;
      rCurrent += rIncrement;
      gCurrent += gIncrement;
      bCurrent += bIncrement;
      framesSinceSkip++;
      framesSinceSkip >= skipNthFrame && (framesSinceSkip = 0, elm.style.backgroundColor = "rgb(" + Math.round(
        rCurrent) + "," + Math.round(gCurrent) + "," + Math.round(bCurrent) + ")");
      currentStep > 0 ? callback = requestAnimationFrame(cycleToTargetColor) : setTargetColor()
    }

    function setPreEndSequence() {
      var n;
      return n = new KeyframeSequencer(document.getElementById("pre_end_sequence")), n.addSequence(document
        .getElementById("color0"), [{
          properties: "-ms-animation-timing-function:linear;opacity:0;",
          duration: 0
        }, {
          properties: "opacity:1;",
          duration: 4
        }], "1s"), n
    }

    function setEndSequence() {
      var n;
      return n = new KeyframeSequencer(document.getElementById("end_sequence")), n.addSequence(document.getElementById(
        "color1"), [{
        properties: "-ms-animation-timing-function:linear;opacity:1;",
        duration: 0
      }], "0s"), n
    }

    function initTouchSequence(n) {
      if (n) initEndSequence(), initEndSequenceMessagesOverlay();
      else {
        initTryThisLaterMessageSequence("6s", "21s", "7s");
        var t, i;
        i = rtl_mode ? document.getElementById("playback_canvas_rtl") : document.getElementById("playback_canvas");
        t = new ScenesManager(i);
        t.addScene(new SceneItem(document.getElementById("touch_sequence"), "s1",
          "from{-ms-animation-timing-function:ease;opacity:0;}15%{opacity:1}85%{opacity:1}to{opacity:1}", "6s",
          setTouchSequence0()));
        t.addScene(new SceneItem(document.getElementById("touch_sequence"), "s1",
          "from{-ms-animation-timing-function:ease;opacity:1;}15%{opacity:1}85%{opacity:1}to{opacity:1}", "7s",
          setTouchSequence1()));
        t.addScene(new SceneItem(document.getElementById("touch_sequence"), "s1",
          "from{-ms-animation-timing-function:ease;opacity:1;}15%{opacity:1}85%{opacity:1}to{opacity:1}", "7s",
          setTouchSequence3()));
        t.addScene(new SceneItem(document.getElementById("touch_sequence"), "s1",
          "from{-ms-animation-timing-function:ease;opacity:1;}15%{opacity:1}85%{opacity:1}to{opacity:1}", "7s",
          setTouchSequence3()));
        t.addScene(new SceneItem(document.getElementById("touch_sequence"), "s1",
          "from{-ms-animation-timing-function:ease;opacity:1;}15%{opacity:1}85%{opacity:1}to{opacity:0}", "7s",
          setTouchSequence2()));
        t.playScenes({
          repeat: !1,
          oncomplete_callback: function () {
            initEndSequence();
            initEndSequenceMessagesOverlay()
          }
        })
      }
    }

    function initTouchAndMouseSequence(n) {
      if (n) initEndSequence(), initEndSequenceMessagesOverlay();
      else {
        initTryThisLaterMessageSequence("6s", "57.4s", "7.8s");
        var t, i;
        i = rtl_mode ? document.getElementById("playback_canvas_rtl") : document.getElementById("playback_canvas");
        t = new ScenesManager(i);
        t.addScene(new SceneItem(document.getElementById("touch_sequence"), "s1",
          "from{-ms-animation-timing-function:ease;opacity:0;}15%{opacity:1}85%{opacity:1}to{opacity:1}", "6s",
          setTouchSequence0()));
        t.addScene(new SceneItem(document.getElementById("touch_sequence"), "s1",
          "from{-ms-animation-timing-function:ease;opacity:1;}15%{opacity:1}85%{opacity:1}to{opacity:1}", "7s",
          setTouchSequence1()));
        t.addScene(new SceneItem(document.getElementById("touch_sequence"), "s1",
          "from{-ms-animation-timing-function:ease;opacity:1;}15%{opacity:1}85%{opacity:1}to{opacity:0}", "7s",
          setTouchSequence2()));
        t.addScene(new SceneItem(document.getElementById("mouse_sequence"), "s2",
          "from{-ms-animation-timing-function:ease;opacity:0;}15%{opacity:1}85%{opacity:1}to{opacity:1}", "9.8s",
          setMouseSequence3()));
        t.addScene(new SceneItem(document.getElementById("mouse_sequence"), "s2",
          "from{-ms-animation-timing-function:ease;opacity:1;}15%{opacity:1}85%{opacity:1}to{opacity:0}", "7.8s",
          setMouseSequence2()));
        t.addScene(new SceneItem(document.getElementById("touch_sequence"), "s1",
          "from{-ms-animation-timing-function:ease;opacity:0;}15%{opacity:1}85%{opacity:1}to{opacity:1}", "9s",
          setTouchSequence4()));
        t.addScene(new SceneItem(document.getElementById("touch_sequence"), "s1",
          "from{-ms-animation-timing-function:ease;opacity:1;}15%{opacity:1}85%{opacity:1}to{opacity:0}", "7s",
          setTouchSequence2()));
        t.addScene(new SceneItem(document.getElementById("mouse_sequence"), "s2",
          "from{-ms-animation-timing-function:ease;opacity:0;}15%{opacity:1}85%{opacity:1}to{opacity:1}", "9.8s",
          setMouseSequence3()));
        t.addScene(new SceneItem(document.getElementById("mouse_sequence"), "s2",
          "from{-ms-animation-timing-function:ease;opacity:1;}15%{opacity:1}85%{opacity:1}to{opacity:0}", "7.8s",
          setMouseSequence2()));
        t.playScenes({
          oncomplete_callback: function () {
            initEndSequence();
            initEndSequenceMessagesOverlay()
          }
        })
      }
    }

    function initMouseSequence(n) {
      if (n) initEndSequence(), initEndSequenceMessagesOverlay();
      else {
        initTryThisLaterMessageSequence("14.3s", "15.6s", "7.8s");
        var t, i;
        i = rtl_mode ? document.getElementById("playback_canvas_rtl") : document.getElementById("playback_canvas");
        t = new ScenesManager(i);
        t.addScene(new SceneItem(document.getElementById("mouse_sequence"), "s2",
          "from{-ms-animation-timing-function:ease;opacity:0;}15%{opacity:1}85%{opacity:1}to{opacity:1}", "14.3s",
          setMouseSequence1()));
        t.addScene(new SceneItem(document.getElementById("mouse_sequence"), "s2",
          "from{-ms-animation-timing-function:ease;opacity:1;}15%{opacity:1}85%{opacity:1}to{opacity:1}", "7.8s",
          setMouseSequence2()));
        t.addScene(new SceneItem(document.getElementById("mouse_sequence"), "s2",
          "from{-ms-animation-timing-function:ease;opacity:1;}15%{opacity:1}85%{opacity:1}to{opacity:1}", "7.8s",
          setMouseSequence2()));
        t.addScene(new SceneItem(document.getElementById("mouse_sequence"), "s2",
          "from{-ms-animation-timing-function:ease;opacity:1;}15%{opacity:1}85%{opacity:1}to{opacity:0}", "7.8s",
          setMouseSequence2()));
        t.playScenes({
          repeat: !1,
          oncomplete_callback: function () {
            initEndSequence();
            initEndSequenceMessagesOverlay()
          }
        })
      }
    }

    function setTouchSequence0() {
      var n;
      return rtl_mode ? (n = new KeyframeSequencer(document.getElementById("touch_sequence")), n.addSequence(document
        .getElementById("touch_intro_text"), [{
          properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
          duration: 0
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:1;",
          duration: 2
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:1;",
          duration: 1
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }], "2s"), n.addSequence(document.getElementById("touch_instruction_text"), [{
        properties: "opacity:0;",
        duration: .01
      }], "0s"), n.addSequence(document.getElementById("touch_arrow"), [{
        properties: "opacity:0;",
        duration: .01
      }], "0s"), n.addSequence(document.getElementById("thumbs"), [{
        properties: "opacity:1;",
        duration: .01
      }], "0s"), n.addSequence(document.getElementById("touch_charms"), [{
        properties: "opacity:0;",
        duration: .01
      }], "0s")) : (n = new KeyframeSequencer(document.getElementById("touch_sequence")), n.addSequence(document
        .getElementById("touch_intro_text"), [{
          properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
          duration: 0
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:1;",
          duration: 2
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:1;",
          duration: 1
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }], "2s"), n.addSequence(document.getElementById("touch_instruction_text"), [{
        properties: "opacity:0;",
        duration: .01
      }], "0s"), n.addSequence(document.getElementById("touch_arrow"), [{
        properties: "opacity:0;",
        duration: .01
      }], "0s"), n.addSequence(document.getElementById("thumbs"), [{
        properties: "opacity:1;",
        duration: .01
      }], "0s"), n.addSequence(document.getElementById("touch_charms"), [{
        properties: "opacity:0;",
        duration: .01
      }], "0s")), n
    }

    function setTouchSequence1() {
      var n;
      return rtl_mode ? (n = new KeyframeSequencer(document.getElementById("touch_sequence")), n.addSequence(document
        .getElementById("touch_intro_text"), [{
          properties: "opacity:0;",
          duration: .01
        }], "0s"), n.addSequence(document.getElementById("touch_instruction_text"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 2
      }], "0s"), n.addSequence(document.getElementById("touch_arrow"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:scale(-1,1) translate(120px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:scale(-1,1) translate(0px,0px);opacity:1;",
        duration: 1
      }, {
        properties: "-ms-transform:scale(-1,1) translate(0px,0px);opacity:0",
        duration: 1.5
      }], "2s"), n.addSequence(document.getElementById("thumbs"), [{
        properties: "-ms-animation-timing-function:ease;-ms-transform:scale(-1,1) rotate(0deg) translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-animation-timing-function:ease;-ms-transform:scale(-1,1) rotate(-20deg) translate(-3px,11px);",
        duration: .25
      }, {
        properties: "-ms-transform:scale(-1,1) rotate(0deg) translate(0px,0px);",
        duration: .75
      }], "4s"), n.addSequence(document.getElementById("touch_charms"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:translate(-35px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);",
        duration: .5
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1",
        duration: 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0",
        duration: 1
      }], "4.05s")) : (n = new KeyframeSequencer(document.getElementById("touch_sequence")), n.addSequence(document
        .getElementById("touch_intro_text"), [{
          properties: "opacity:0;",
          duration: .01
        }], "0s"), n.addSequence(document.getElementById("touch_instruction_text"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 2
      }], "0s"), n.addSequence(document.getElementById("touch_arrow"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:translate(120px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0",
        duration: 1.5
      }], "2s"), n.addSequence(document.getElementById("thumbs"), [{
        properties: "-ms-animation-timing-function:ease;-ms-transform:rotate(0deg) translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-animation-timing-function:ease;-ms-transform:rotate(-20deg) translate(-3px,11px);",
        duration: .25
      }, {
        properties: "-ms-transform:rotate(0deg) translate(0px,0px);",
        duration: .75
      }], "4s"), n.addSequence(document.getElementById("touch_charms"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:translate(35px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);",
        duration: .5
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1",
        duration: 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0",
        duration: 1
      }], "4.05s")), n
    }

    function setTouchSequence2() {
      var n;
      return rtl_mode ? (n = new KeyframeSequencer(document.getElementById("touch_sequence")), n.addSequence(document
        .getElementById("touch_intro_text"), [{
          properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
          duration: 0
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 2
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }], "0s"), n.addSequence(document.getElementById("touch_instruction_text"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:1;-ms-transform:translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 2
      }], "0s"), n.addSequence(document.getElementById("touch_arrow"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:scale(-1,1) translate(120px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:scale(-1,1) translate(0px,0px);opacity:1;",
        duration: 1
      }, {
        properties: "-ms-transform:scale(-1,1) translate(0px,0px);opacity:0",
        duration: 1.5
      }], "2s"), n.addSequence(document.getElementById("thumbs"), [{
        properties: "-ms-animation-timing-function:ease;-ms-transform:scale(-1,1) rotate(0deg) translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-animation-timing-function:ease;-ms-transform:scale(-1,1) rotate(-20deg) translate(-3px,11px);",
        duration: .25
      }, {
        properties: "-ms-transform:scale(-1,1) rotate(0deg) translate(0px,0px);",
        duration: .75
      }], "4s"), n.addSequence(document.getElementById("touch_charms"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:translate(-35px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);",
        duration: .5
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1",
        duration: 1
      }], "4.05s")) : (n = new KeyframeSequencer(document.getElementById("touch_sequence")), n.addSequence(document
        .getElementById("touch_intro_text"), [{
          properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
          duration: 0
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 2
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }], "0s"), n.addSequence(document.getElementById("touch_instruction_text"), [{
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 2
      }], "0s"), n.addSequence(document.getElementById("touch_arrow"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:translate(120px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0",
        duration: 1.5
      }], "2s"), n.addSequence(document.getElementById("thumbs"), [{
        properties: "-ms-animation-timing-function:ease;-ms-transform:rotate(0deg) translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-animation-timing-function:ease;-ms-transform:rotate(-20deg) translate(-3px,11px);",
        duration: .25
      }, {
        properties: "-ms-transform:rotate(0deg) translate(0px,0px);",
        duration: .75
      }], "4s"), n.addSequence(document.getElementById("touch_charms"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:translate(35px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);",
        duration: 1
      }], "4.05s")), n
    }

    function setTouchSequence3() {
      var n;
      return rtl_mode ? (n = new KeyframeSequencer(document.getElementById("touch_sequence")), n.addSequence(document
        .getElementById("touch_intro_text"), [{
          properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
          duration: 0
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 2
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }], "0s"), n.addSequence(document.getElementById("touch_instruction_text"), [{
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 2
      }], "0s"), n.addSequence(document.getElementById("touch_arrow"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:scale(-1,1) translate(120px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:scale(-1,1) translate(0px,0px);opacity:1;",
        duration: 1
      }, {
        properties: "-ms-transform:scale(-1,1) translate(0px,0px);opacity:0",
        duration: 1.5
      }], "2s"), n.addSequence(document.getElementById("thumbs"), [{
        properties: "-ms-animation-timing-function:ease;-ms-transform:scale(-1,1) rotate(0deg) translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-animation-timing-function:ease;-ms-transform:scale(-1,1) rotate(-20deg) translate(-3px,11px);",
        duration: .25
      }, {
        properties: "-ms-transform:scale(-1,1) rotate(0deg) translate(0px,0px);",
        duration: .75
      }], "4s"), n.addSequence(document.getElementById("touch_charms"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:translate(-35px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);",
        duration: .5
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1",
        duration: 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0",
        duration: 1
      }], "4.05s")) : (n = new KeyframeSequencer(document.getElementById("touch_sequence")), n.addSequence(document
        .getElementById("touch_intro_text"), [{
          properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
          duration: 0
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 2
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }], "0s"), n.addSequence(document.getElementById("touch_instruction_text"), [{
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 2
      }], "0s"), n.addSequence(document.getElementById("touch_arrow"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:translate(120px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0",
        duration: 1.5
      }], "2s"), n.addSequence(document.getElementById("thumbs"), [{
        properties: "-ms-animation-timing-function:ease;-ms-transform:rotate(0deg) translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-animation-timing-function:ease;-ms-transform:rotate(-20deg) translate(-3px,11px);",
        duration: .25
      }, {
        properties: "-ms-transform:rotate(0deg) translate(0px,0px);",
        duration: .75
      }], "4s"), n.addSequence(document.getElementById("touch_charms"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:translate(35px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);",
        duration: .5
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1",
        duration: 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0",
        duration: 1
      }], "4.05s")), n
    }

    function setTouchSequence4() {
      var n;
      return rtl_mode ? (n = new KeyframeSequencer(document.getElementById("touch_sequence")), n.addSequence(document
        .getElementById("touch_intro_text"), [{
          properties: "opacity:0;",
          duration: .01
        }], "0s"), n.addSequence(document.getElementById("touch_instruction_text"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 2
      }], "2s"), n.addSequence(document.getElementById("touch_arrow"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:scale(-1,1) translate(120px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:scale(-1,1) translate(0px,0px);opacity:1;",
        duration: 1
      }, {
        properties: "-ms-transform:scale(-1,1) translate(0px,0px);opacity:0",
        duration: 1.5
      }], "4s"), n.addSequence(document.getElementById("thumbs"), [{
        properties: "-ms-animation-timing-function:ease;-ms-transform:scale(-1,1) rotate(0deg) translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-animation-timing-function:ease;-ms-transform:scale(-1,1) rotate(-20deg) translate(-3px,11px);",
        duration: .25
      }, {
        properties: "-ms-transform:scale(-1,1) rotate(0deg) translate(0px,0px);",
        duration: .75
      }], "6s"), n.addSequence(document.getElementById("touch_charms"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:translate(-35px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);",
        duration: .5
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1",
        duration: 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0",
        duration: 1
      }], "6.05s")) : (n = new KeyframeSequencer(document.getElementById("touch_sequence")), n.addSequence(document
        .getElementById("touch_intro_text"), [{
          properties: "opacity:0;",
          duration: .01
        }], "0s"), n.addSequence(document.getElementById("touch_instruction_text"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 2
      }], "2s"), n.addSequence(document.getElementById("touch_arrow"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:translate(120px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0",
        duration: 1.5
      }], "4s"), n.addSequence(document.getElementById("thumbs"), [{
        properties: "-ms-animation-timing-function:ease;-ms-transform:rotate(0deg) translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-animation-timing-function:ease;-ms-transform:rotate(-20deg) translate(-3px,11px);",
        duration: .25
      }, {
        properties: "-ms-transform:rotate(0deg) translate(0px,0px);",
        duration: .75
      }], "6s"), n.addSequence(document.getElementById("touch_charms"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:translate(35px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);",
        duration: .5
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1",
        duration: 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0",
        duration: 1
      }], "6.05s")), n
    }

    function setMouseSequence1() {
      var n;
      return rtl_mode ? (n = new KeyframeSequencer(document.getElementById("mouse_sequence")), n.addSequence(document
        .getElementById("mouse_intro_text"), [{
          properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
          duration: 0
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:1;",
          duration: 2
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:1;",
          duration: 1
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }], "2s"), n.addSequence(document.getElementById("mouse_instruction_text"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 2
      }], "6s"), n.addSequence(document.getElementById("cursor"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;",
        duration: 0
      }, {
        properties: "opacity:1;-ms-transform:translate(0px,0px)",
        duration: 2
      }, {
        properties: "opacity:1;-ms-transform:translate(0px,0px)",
        duration: 1
      }, {
        properties: "opacity:1;-ms-transform:translate(-130px,-130px)",
        duration: 1
      }], "8s"), n.addSequence(document.getElementById("mouse_arrow"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px)",
        duration: 0
      }, {
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:1;-ms-transform:translate(0px,0px);",
        duration: 1
      }, {
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:1;-ms-transform:translate(0px,0px);",
        duration: .8
      }, {
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;",
        duration: .7
      }], "9s"), n.addSequence(document.getElementById("mouse_charms"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:translate(-35px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(4px,0px);",
        duration: .5
      }, {
        properties: "-ms-transform:translate(4px,0px);opacity:1",
        duration: 1
      }, {
        properties: "-ms-transform:translate(4px,0px);opacity:0",
        duration: 1
      }], "11.6s")) : (n = new KeyframeSequencer(document.getElementById("mouse_sequence")), n.addSequence(document
        .getElementById("mouse_intro_text"), [{
          properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
          duration: 0
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:1;",
          duration: 2
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:1;",
          duration: 1
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }], "2s"), n.addSequence(document.getElementById("mouse_instruction_text"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 2
      }], "6s"), n.addSequence(document.getElementById("cursor"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;",
        duration: 0
      }, {
        properties: "opacity:1;-ms-transform:translate(0px,0px)",
        duration: 2
      }, {
        properties: "opacity:1;-ms-transform:translate(0px,0px)",
        duration: 1
      }, {
        properties: "opacity:1;-ms-transform:translate(100px,-100px)",
        duration: 1
      }], "8s"), n.addSequence(document.getElementById("mouse_arrow"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px)",
        duration: 0
      }, {
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:1;-ms-transform:translate(0px,0px);",
        duration: 1
      }, {
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:1;-ms-transform:translate(0px,0px);",
        duration: .8
      }, {
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;",
        duration: .7
      }], "9s"), n.addSequence(document.getElementById("mouse_charms"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:translate(35px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);",
        duration: .5
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1",
        duration: 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0",
        duration: 1
      }], "11.6s")), n
    }

    function setMouseSequence3() {
      var n;
      return rtl_mode ? (n = new KeyframeSequencer(document.getElementById("mouse_sequence")), n.addSequence(document
        .getElementById("mouse_intro_text"), [{
          properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
          duration: 0
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 2
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }], "0s"), n.addSequence(document.getElementById("mouse_instruction_text"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 2
      }], "2s"), n.addSequence(document.getElementById("cursor"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;",
        duration: 0
      }, {
        properties: "opacity:1;-ms-transform:translate(0px,0px)",
        duration: 2
      }, {
        properties: "opacity:1;-ms-transform:translate(0px,0px)",
        duration: 1
      }, {
        properties: "opacity:1;-ms-transform:translate(-130px,-130px)",
        duration: 1
      }], "4s"), n.addSequence(document.getElementById("mouse_arrow"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px)",
        duration: 0
      }, {
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:1;-ms-transform:translate(0px,0px);",
        duration: 1
      }, {
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:1;-ms-transform:translate(0px,0px);",
        duration: .8
      }, {
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;",
        duration: .7
      }], "5s"), n.addSequence(document.getElementById("mouse_charms"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:translate(-35px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(4px,0px);",
        duration: .5
      }, {
        properties: "-ms-transform:translate(4px,0px);opacity:1",
        duration: 1
      }, {
        properties: "-ms-transform:translate(4px,0px);opacity:0",
        duration: 1
      }], "7.6s")) : (n = new KeyframeSequencer(document.getElementById("mouse_sequence")), n.addSequence(document
        .getElementById("mouse_intro_text"), [{
          properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
          duration: 0
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 2
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }], "0s"), n.addSequence(document.getElementById("mouse_instruction_text"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 2
      }], "2s"), n.addSequence(document.getElementById("cursor"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;",
        duration: 0
      }, {
        properties: "opacity:1;-ms-transform:translate(0px,0px)",
        duration: 2
      }, {
        properties: "opacity:1;-ms-transform:translate(0px,0px)",
        duration: 1
      }, {
        properties: "opacity:1;-ms-transform:translate(100px,-100px)",
        duration: 1
      }], "4s"), n.addSequence(document.getElementById("mouse_arrow"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px)",
        duration: 0
      }, {
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:1;-ms-transform:translate(0px,0px);",
        duration: 1
      }, {
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:1;-ms-transform:translate(0px,0px);",
        duration: .8
      }, {
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;",
        duration: .7
      }], "5s"), n.addSequence(document.getElementById("mouse_charms"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:translate(35px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);",
        duration: .5
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1",
        duration: 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0",
        duration: 1
      }], "7.6s")), n
    }

    function setMouseSequence2() {
      var n;
      return rtl_mode ? (n = new KeyframeSequencer(document.getElementById("mouse_sequence")), n.addSequence(document
        .getElementById("mouse_intro_text"), [{
          properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
          duration: 0
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 2
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }], "0s"), n.addSequence(document.getElementById("mouse_instruction_text"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:1;-ms-transform:translate(0px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 2
      }], "0s"), n.addSequence(document.getElementById("cursor"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;",
        duration: 0
      }, {
        properties: "opacity:1;-ms-transform:translate(0px,0px)",
        duration: 2
      }, {
        properties: "opacity:1;-ms-transform:translate(0px,0px)",
        duration: 1
      }, {
        properties: "opacity:1;-ms-transform:translate(-130px,-130px)",
        duration: 1
      }], "2s"), n.addSequence(document.getElementById("mouse_arrow"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px)",
        duration: 0
      }, {
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:1;-ms-transform:translate(0px,0px);",
        duration: 1
      }, {
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:1;-ms-transform:translate(0px,0px);",
        duration: .8
      }, {
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;",
        duration: .7
      }], "3s"), n.addSequence(document.getElementById("mouse_charms"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:translate(-35px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(4px,0px);",
        duration: .5
      }, {
        properties: "-ms-transform:translate(4px,0px);opacity:1",
        duration: 1
      }, {
        properties: "-ms-transform:translate(4px,0px);opacity:0",
        duration: 1
      }], "5.6s")) : (n = new KeyframeSequencer(document.getElementById("mouse_sequence")), n.addSequence(document
        .getElementById("mouse_intro_text"), [{
          properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
          duration: 0
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 2
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }], "0s"), n.addSequence(document.getElementById("mouse_instruction_text"), [{
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1;",
        duration: 2
      }], "0s"), n.addSequence(document.getElementById("cursor"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;",
        duration: 0
      }, {
        properties: "opacity:1;-ms-transform:translate(0px,0px)",
        duration: 2
      }, {
        properties: "opacity:1;-ms-transform:translate(0px,0px)",
        duration: 1
      }, {
        properties: "opacity:1;-ms-transform:translate(100px,-100px)",
        duration: 1
      }], "2s"), n.addSequence(document.getElementById("mouse_arrow"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px)",
        duration: 0
      }, {
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:1;-ms-transform:translate(0px,0px);",
        duration: 1
      }, {
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:1;-ms-transform:translate(0px,0px);",
        duration: .8
      }, {
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;",
        duration: .7
      }], "3s"), n.addSequence(document.getElementById("mouse_charms"), [{
        properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);-ms-transform:translate(35px,0px);",
        duration: 0
      }, {
        properties: "-ms-transform:translate(0px,0px);",
        duration: .5
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:1",
        duration: 1
      }, {
        properties: "-ms-transform:translate(0px,0px);opacity:0",
        duration: 1
      }], "5.6s")), n
    }

    function init() {
      document.getElementById("playback_canvas" + (rtl_mode ? "_rtl" : "")).style.visibility = "visible";
      centerPos();
      window.onresize = function () {
        centerPos()
      }
    }

    function adjustInstructionText() {
      for (var t, u, r, i = window.innerWidth, f = 564, e = document.querySelectorAll(".instruction_text"), n = 0; n < e
        .length; n++) t = e[n], t.style.width = i + "px", t.style.left = (f - i) / 2 + "px", t.style.top = -t
        .clientHeight - 40 + "px";
      for (u = document.querySelectorAll(".instruction_text_small"), n = 0; n < u.length; n++) r = u[n], r.style.width =
        i + "px", r.style.left = (f - i) / 2 + "px", r.style.top = "390px"
    }

    function centerPos() {
      var f, e, n, t;
      adjustInstructionText();
      var o = 564,
        s = 390,
        i = window.innerWidth,
        r = window.innerHeight,
        h = document.querySelector("#playback_canvas" + (rtl_mode ? "_rtl" : ""));
      h.style.left = (i - o) / 2 + "px";
      h.style.top = (r - s) / 2 + v_offset + "px";
      f = document.querySelector("#trythislater_overlay_canvas");
      f.style.left = (i - o) / 2 + "px";
      f.style.top = (r - s) / 2 + v_offset + "px";
      var u = document.querySelectorAll(".final_text"),
        i = window.innerWidth,
        r = window.innerHeight;
      for (n = 0; n < u.length; n++) t = u[n], t.style.width = i + "px", t.style.top = (r - t.offsetHeight) / 2 + "px";
      var u = document.querySelectorAll(".final_text_small"),
        i = window.innerWidth,
        r = window.innerHeight;
      for (n = 0; n < u.length; n++) t = u[n], t.style.width = i + "px", t.style.top = Math.floor((r - t.offsetHeight +
        120) / 2) + "px";
      for (e = document.querySelectorAll(".fullscreen_color"), n = 0; n < e.length; n++) t = e[n], t.style.width =
        window.outerWidth + "px", t.style.height = window.outerHeight + "px"
    }

    function endAnimation() {
      var n, t = document.getElementById("drape");
      n = new ScenesManager(t);
      n.addScene(new SceneItem(document.getElementById("wrap_up_sequence"), "ws", "to{opacity:1}", "5.2s",
        setWrapUpSequence()));
      n.playScenes();
      setTimeout(function () {
        for (var i, t = document.querySelector("#container").childNodes, n = 0; n < t.length; n++) i = t[n],
          document.querySelector("#container").removeChild(i)
      }, 2e3)
    }

    function setWrapUpSequence() {
      var n;
      return n = new KeyframeSequencer(document.getElementById("wrap_up_sequence")), n.addSequence(document
        .getElementById("finish_text"), [{
          properties: "-ms-animation-timing-function:cubic-bezier(0.1, 0.9, 0.2, 1);opacity:0;-ms-transform:translate(0px,0px);",
          duration: 0
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:1;",
          duration: 1
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:1;",
          duration: 2
        }, {
          properties: "-ms-transform:translate(0px,0px);opacity:0;",
          duration: 1
        }], "1s"), n.addSequence(document.getElementById("black_panel"), [{
        properties: "-ms-animation-timing-function:linear;opacity:0;",
        duration: 0
      }, {
        properties: "opacity:1;",
        duration: 1
      }], "0s"), n
    }

    function setUserColor(n, t) {
      for (var i, u = document.getElementsByClassName("user_color1"), r = 0; r < u.length; r++) i = u[r], i.attributes
        .fill ? i.attributes.fill.value = n : i.style.backgroundColor = n;
      for (u = document.getElementsByClassName("user_color2"), r = 0; r < u.length; r++) i = u[r], i.attributes.fill ? i
        .attributes.fill.value = t : i.style.backgroundColor = t
    }

    function setItemText(n, t) {
      document.getElementById(n).innerHTML = t
    }

    function setAriaLabel(n, t) {
      document.getElementById(n).setAttribute("aria-label", t)
    }
    var rtl_mode = !1,
      v_offset = 40,
      zdpMessageIndex = 0,
      rgZDPMessages = [],
      elm, rCurrent, gCurrent, bCurrent, rIncrement, gIncrement, bIncrement, steps = 240,
      currentStep = 240,
      skipNthFrame = 3,
      framesSinceSkip = 0,
      numberColors = 2,
      currentColor = 0,
      targetColor = 1,
      rValues = [0, 0],
      gValues = [0, 0],
      bValues = [0, 0],
      callback = null
  </script>
  
  <link rel="stylesheet" href="index.d2510029.css">
</head>
<body>
  <div id="container">
  </div>
  <div id="final_messages_overlay_canvas"
    style="/* visibility: hidden; */position: unset;text-align: center;font-size: 40pt;font-family: Segoe UI;vertical-align: middle;height: 400px;">
    <div id="message_overlay_sequence">
      <div class="center">
        <div id="final_message_text1" aria-hidden="true">
          The Website AeroGlass is under maintenance.
        </div>
        <div id="sub_final_message_text1" aria-hidden="true" class="final_text_small">
          Some things will maybe be uploaded to GitHub at @WishiiTelly!
        </div>
      </div>
    </div>
  </div>
  <div id="zdp_final_messages_overlay_canvas" style="visibility: hidden; position: absolute; top: 0px; left: 0px">
    <div id="zdp_message_overlay_sequence">
      <div class="final_text">
        <div id="zdp_final_message_text" aria-hidden="true">
          [ZDP Installing Updates]
        </div>
      </div>
      <div id="zdp_sub_final_message_text" class="final_text_small" aria-hidden="true">
        [ZDP Don't turn off your PC]
      </div>
    </div>
  </div>
  <div class="circles">
    <div class="circle1"></div>
    <div class="circle2"></div>
    <div class="circle3"></div>
  </div>
  <div id="drape" style="visibility: hidden">
    <div id="wrap_up_sequence">
      <div id="black_panel" class="fullscreen_color user_color1" style="background-color: rgb(0,0,0)"></div>
      <div id="finish_text" class="final_text" style="color:#fff" aria-hidden="true">
        [Let's start]
      </div>
    </div>
  </div>
</body>
</html>
