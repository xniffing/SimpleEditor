<template>
    <section>
        <codemirror class="editor"
                ref="myCm"
                :value="code"
                :options="cmOptions"
                @ready="onCmReady"
                @focus="onCmFocus"
                @input="onCmCodeChange"
        >
        </codemirror>
    </section>
</template>

<script>
    // language js
    import "codemirror/mode/javascript/javascript.js";
    // theme css
    import "codemirror/theme/base16-dark.css";
    // more codemirror resources
    // import 'codemirror/some-resource...'
    export default {
        data() {
            return {
                code: ` ---
- hosts: webservers

  vars:
    http_port: 80
    max_clients: 200

  remote_user: root

  tasks:
  - name: ensure apache is at the latest version
    yum:
      name: httpd
      state: latest

  - name: write the apache config file
    template:
      src: /srv/httpd.j2
      dest: /etc/httpd.conf
    notify:
    - restart apache

  - name: ensure apache is running
    service:
      name: httpd
      state: started

  handlers:
    - name: restart apache
      service:
        name: httpd
        state: restarted `,
                cmOptions: {
                    // codemirror options
                    tabSize: 4,
                    mode: "text/javascript",
                    theme: "base16-dark",
                    lineNumbers: true,
                    line: true
                    // more codemirror options, 更多 codemirror 的高级配置...
                }
            };
        },
        methods: {
            onCmReady(cm) {
                console.log("the editor is readied!", cm);
            },
            onCmFocus(cm) {
                console.log("the editor is focus!", cm);
            },
            onCmCodeChange(newCode) {
                console.log("this is new code", newCode);
                this.code = newCode;
            }
        },
        computed: {
            codemirror() {
                return this.$refs.myCm.codemirror;
            }
        },
        mounted() {
            console.log("this is current codemirror object", this.codemirror);
            // you can use this.codemirror to do something...
        }
    };
</script>

<style>

    body, html {
        margin: 0;
        height: 100%;
    }
    .CodeMirror.cm-s-base16-dark{
        height: 100vh;
    }


</style>
