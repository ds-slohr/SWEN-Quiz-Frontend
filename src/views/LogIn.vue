<template>
    <form @submit="onSubmit" class="add-form">
        <div class="form-control">
            <label>Nickname:</label>
            <input type="text" v-model="username" name="username" placeholder="Add NickName" />
            <label>Password:</label>
            <input type="password" v-model="password" name="password" />
        </div>
        <input type="submit" value="Submit" class="btn btn-block btn-submit" />
        <button  @click="useDemo"  class="btn btn-block btn-submit">Use Demo</button>
    </form>
</template>

<script>

export default {
    name: 'Login-Page',
    data() {
        return {
            username: String,
            password: String
        }
    },
    methods: {
        async onSubmit(e) {
            e.preventDefault()

            const res = await fetch(`https://swen-quiz-backend.azurewebsites.net/auth/login/`, {
                method: "POST",
                headers: { 'Content-Type': 'application/json;charset=utf-8' },
                body: JSON.stringify(
                    {
                        'username': this.username,
                        'password': this.password
                    })
            })
            const data = await res.json()

            if (data.token) {
                this.$emit('setUser', this.username)
                this.$emit('setToken', data.token)
                this.$emit('setAdmin', true)
                this.$router.push(`/admin`)
            } else {
                alert("invalid Username or Password")
            }
        },
        async useDemo() {
            this.username = 'DEMO'
            this.password = 'DEMO'
        }
    },
    created() {
        this.username = null
        this.password = null
    }
}
</script>

<style scoped>
.form-control {
    margin: 20px 0;
}

.form-control label {
    display: block;
}

.form-control input {
    width: 100%;
    height: 40px;
    padding: 5px 7px 5px 15px;
    font-size: 17px;
    border-radius: 15px;
}

.form-control-check {
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.form-control-check label {
    flex: 1;
}

.form-control-check input {
    flex: 2;
    height: 20px;
}

.fas {
    color: green;
    cursor: pointer;
}

h2 {
    color: white;
    text-align: center;
    font-weight: normal;
    margin-bottom: 50px;
    font-family: 'CaviarDreams';
}

.qr-code {
    margin: 0 auto;
    display: block;
}

label {
    color: white;
    padding-left: 15px;
    margin-bottom: 10px;
}

.btn-submit {
    background: #0071bc;
}

.btn-start {
    margin-top: 60px;
}
</style>