<template>
 <main class="main" id="main">
     <section class="section profile">
      <div class="row">
        <div class="col-xl-4">

          <div class="card">
            <div class="card-body profile-card pt-4 d-flex flex-column align-items-center">
              <img src="/template/assets/img/profile.png" alt="Profile" class="rounded-circle">
              <h2>{{info.studentName}}</h2>
              <h3>{{info.progShortName}}</h3>
              <h3>ID: {{info.studentId}}</h3>
            </div>
          </div>

           <router-link to="/" class="btn btn-success btn-sm mb-2">Back to Home</router-link>

        </div>

        <div class="col-xl-8">

          <div class="card">
            <div class="card-body pt-3">
              <!-- Bordered Tabs -->
              <ul class="nav nav-tabs nav-tabs-bordered">

                <li class="nav-item">
                  <button @click="viewCgpa(parseInt(semesterYear)+''+1)" class="nav-link active" data-bs-toggle="tab">1st Semester</button>
                </li>  
                <li class="nav-item">
                  <button @click="viewCgpa(parseInt(semesterYear)+''+2)" class="nav-link" data-bs-toggle="tab">2nd Semester</button>
                </li>
                <li class="nav-item">
                  <button @click="viewCgpa(parseInt(semesterYear)+''+3)" class="nav-link" data-bs-toggle="tab">3rd Semester</button>
                </li>  
                <li class="nav-item">
                  <button @click="viewCgpa(parseInt(semesterYear+1)+''+1)" class="nav-link" data-bs-toggle="tab">4th Semester</button>
                </li>  
                <li class="nav-item">
                  <button @click="viewCgpa(parseInt(semesterYear+1)+''+2)" class="nav-link" data-bs-toggle="tab">5th Semester</button>
                </li>  
                <li class="nav-item">
                  <button @click="viewCgpa(parseInt(semesterYear+1)+''+3)" class="nav-link" data-bs-toggle="tab">6th Semester</button>
                </li> 
                <li class="nav-item">
                  <button @click="viewCgpa(parseInt(semesterYear+2)+''+1)" class="nav-link" data-bs-toggle="tab">7th Semester</button>
                </li>  
                <li class="nav-item">
                  <button @click="viewCgpa(parseInt(semesterYear+2)+''+2)" class="nav-link" data-bs-toggle="tab">8th Semester</button>
                </li>  
                <li class="nav-item">
                  <button @click="viewCgpa(parseInt(semesterYear+2)+''+3)" class="nav-link" data-bs-toggle="tab">9th Semester</button>
                </li>
                                <li class="nav-item">
                  <button @click="viewCgpa(parseInt(semesterYear+3)+''+1)" class="nav-link" data-bs-toggle="tab">10th Semester</button>
                </li>  
                <li class="nav-item">
                  <button @click="viewCgpa(parseInt(semesterYear+3)+''+2)" class="nav-link" data-bs-toggle="tab">11th Semester</button>
                </li>  
                <li class="nav-item">
                  <button @click="viewCgpa(parseInt(semesterYear+3)+''+3)" class="nav-link" data-bs-toggle="tab">12th Semester</button>
                </li>    

              </ul>

              <div class="tab-content pt-2">

              
              <div v-if="semester.length<1">
                  <h6 class="card-title text-danger">Not pass this semester</h6>
              </div>
              <div v-else v-for="item in semester" :key="item.courseId" class="tab-pane fade show active">
                  <h5 class="card-title">{{item.courseTitle}}</h5>

                  <div class="row">
                    <div class="col-6 label ">Subject Code</div>
                    <div class="col-6">{{item.customCourseId}}</div>
                  </div>
                  <div class="row">
                    <div class="col-6 label ">GPA</div>
                    <div class="col-6">{{item.pointEquivalent}}</div>
                  </div>
                  <div class="row">
                    <div class="col-6 label ">Grade</div>
                    <div class="col-6">{{item.gradeLetter}}</div>
                  </div>
                  <div class="row">
                    <div class="col-6 label ">Semester GPA</div>
                    <div class="col-6">{{item.cgpa}}</div>
                  </div>
                  <div class="row">
                    <div class="col-6 label ">Credit</div>
                    <div class="col-6">{{item.totalCredit}}</div>
                  </div>

                </div>
              </div><!-- End Bordered Tabs -->

            </div>
          </div>

        </div>
      </div>
    </section>
 </main>
</template>

<script>
import axios from 'axios'
export default{
    name:'CgpaPage',
    data(){
        return{
            info: '',
            semesterYear: '',
            semesterId: '',
            semester: ''
        }
    },
    mounted(){
        var self = this
        let studentId = this.$route.params.studentId;
        axios.get('https://diu-api.herokuapp.com/studentId='+studentId)
        .then(function (response) {
            if(response.data.studentId==null){
              alert('You have not passed in any semester!')
            }else{
              self.info = response.data
              self.semesterId = response.data.semesterId;
              self.semesterYear = self.semesterId / 10;   
              axios.get('https://diu-api.herokuapp.com/studentId='+ self.info.studentId +'/semesterId='+self.info.semesterId)
              .then(function (res) {
              self.semester = res.data
              })
              .catch(function(err){
              console.log(err)
        })
            }
        })
        .catch(function (error) {
            console.log(error);
         })
    },
    methods: {
      viewCgpa(id){
        var self = this
              axios.get('https://diu-api.herokuapp.com/studentId='+ self.info.studentId +'/semesterId='+id)
            .then(function (res) {
            self.semester = res.data
            })
            .catch(function(err){
            console.log(err)
        })
      }
    }
}
</script>
    
