<template>
    <section class="form-sec">
        <div class="row">
            <div class="col-xl-3 col-lg-2 col-md-1">

            </div>
            <div class="col-xl-6 col-lg-8 col-md-10 col-sm-12">
                <div class="form-div">
                    <form id="love_form" @submit.prevent="submitForm">
                        <div class="row">
                            <div class="col-lg-5 col-md-5 col-sm-12">
                                <label for="name1">Your Name: </label>
                            </div>
                            <div class="col-lg-7 col-md-7 col-sm-12">
                                <input v-model="name1" type="text" class="form-control" name="name1" id="name1" required>
                                <span class="errorMsg" id="errorMsg1"></span>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-lg-5 col-md-5 col-sm-12">
                                <label for="name2">Partner's Name: </label>
                            </div>
                            <div class="col-lg-7 col-md-7 col-sm-12">
                                <input v-model="name2" type="text" class="form-control" name="name2" id="name2" required>
                                <span class="errorMsg" id="errorMsg2"></span>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-lg-5 col-md-5 col-sm-12">
                            </div>
                            <div class="col-lg-7 col-md-7 col-sm-12">
                                <input type="submit" class="form-control" name="submit" id="submit" value="Check %">
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-12">
                                <label class="result">
                                    {{ msg }}
                                </label>
                            </div>
                        </div>
                    </form>
                </div>
            </div>
            <div class="col-xl-3 col-lg-2 col-md-10">

            </div>
        </div>
    </section>
</template>

<script>
    import { ref } from 'vue'
    export default {
        data() {
            return {
                name1: '',
                name2: '',

                msg: ref(''),
                per: ref(0),
            }
        },
        methods: {
            submitForm() {
                const formData = {
                    name1: this.name1,
                    name2: this.name2,
                }

                this.per = this.getPercentage(formData.name1, formData.name2);
                this.msg = formData.name1+' Loves '+formData.name2+': '+this.per+'%';
            },
            getPercentage(name1, name2)
            {
                let full = name1.toLowerCase()+" loves "+name2.toLowerCase();
                let count = new Array(full.length);
                let arr = full.split('');

                for (let i = 0; i < arr.length; i++) {
                    if (arr[i] === "-" || arr[i] === ")" || arr[i] === "(" || arr[i] === " ")
                        continue;

                    if (arr[i] !== "0")
                        count[i] = 1;

                    for (let j = i + 1; j < arr.length; j++) {
                        if (arr[i] === arr[j] && arr[j] !== "0") {
                            count[i] = count[i] + 1;
                            arr[j] = "0";
                        }
                    }
                }
                // console.log(arr);
                let cnt = Object.values(count);
                let s = cnt.length;
                // console.log(cnt);
                while(s>2) {
                    let temp = new Array();
                    let j = s-1;

                    for (let i=0; i < (s/2); i++) { 
                        if(j <= i)
                            temp[i] = cnt[i];
                        else
                            temp[i] = cnt[i] + cnt[j];
                        j--;
                    }

                    cnt = Object.values(temp);
                    s = cnt.length;
                    temp = undefined;
                    // console.log(cnt);
                }
                let result  = cnt.join("");

                if(result.length > 2) {
                    if(result.length == 3) {
                        if(parseInt(result, 10) == 100)
                            return result;
                        else {
                            let temp = new Array();
                            temp[0] = parseInt(result[0])+parseInt(result[2]);
                            temp[1] = parseInt(result[1]);
                            result = temp.join("");
                            return result;
                        }
                    }
                    else if(result.length == 4) {
                        let temp = new Array();
                        temp[0] = parseInt(result[0])+parseInt(result[3]);
                        temp[1] = parseInt(result[1])+parseInt(result[2]);
                        result = temp.join("");
                        return result;
                    }

                } else return result;
            }
        }
    }
</script>