<template>
<div>
	<!-- 导航栏路径跳转返回首页 -->
	<div class="positionBar">
		<span>您的当前位置：</span>
		<span><a href="#/login/main/eduAdminHome" class="returnHome">首页</a></span>
		<span> > 教师课表</span>
	</div>
	<div class="tableSelect">
		<select v-model="selSemester">
			<option disabled value="">选择学期</option>
			<option v-for="semesterOpt in yearSemester" :value="semesterOpt" >
				{{semesterOpt}}
				<!-- {{ semesterOpt.year }}第{{ semesterOpt.term }}学期 -->
			</option>
		</select>
		<select v-model="selWeek">
			<option disabled value="">选择周数</option>
			<option v-for="weekOpt in week" :value="weekOpt">
				第{{ weekOpt }}周
			</option>
		</select>
		<button class="am-btn am-btn-success am-radius" @click="checkTableBtn()">查看课表</button>
	</div>

	<div id="timetable">
		<!-- 教师课表 -->
		<div class="timetableBody">
			<table class="normalTable">
				<thead>
					<tr>
						<th width="10%">节次/周次</th>
						<th width="14.8%">星期一</th>
						<th width="14.8%">星期二</th>
						<th width="14.8%">星期三</th>
						<th width="14.8%">星期四</th>
						<th width="14.8%">星期五</th>
					</tr>
				</thead>
				<tbody>
					<tr>
						<td>第一节<br>8:30-9:15</td>
						<td v-html="firstCourse" rowspan="2"></td>
						<td v-html="fourthCourse" rowspan="2"></td>
						<td v-html="seventhCourse" rowspan="2"></td>
						<td v-html="tenthCourse" rowspan="2"></td>
						<td v-html="thirteenthCourse" rowspan="2"></td>
					</tr>
					<tr>
						<td>第二节<br>9:20-10:05</td>
					</tr>
					<tr>
						<td>第三节<br>10:20-11:05</td>
						<td v-html="secondCourse" rowspan="2"></td>
						<td v-html="fifthCourse" rowspan="2"></td>
						<td v-html="eightCourse" rowspan="2"></td>
						<td v-html="eleventhCourse" rowspan="2"></td>
						<td v-html="fourteenthCourse" rowspan="2"></td>
					</tr>
					<tr>
						<td>第四节<br>11:10-11:55</td>
					</tr>
					<tr>
						<td>第五节<br>14:30-15:15</td>
						<td v-html="thirdCourse" rowspan="2"></td>
						<td v-html="sixthCourse" rowspan="2"></td>
						<td v-html="ninthCourse" rowspan="2"></td>
						<td v-html="twelfthCourse" rowspan="2"></td>
						<td v-html="classmeeting" rowspan="2">班会</td>
					</tr>
					<tr>
						<td>第六节<br>15:20-16:05</td>
					</tr>
				</tbody>
			</table>
		</div>
		<!-- 所有课程信息列表 -->
		<div class="timetableList">
			<table class="normalTable">
				<thead>
					<tr>
						<th width="">课程编码</th>
						<th width="">课程名称</th>
						<th width="">课程序号</th>
						<th width="">教师</th>
					</tr>
				</thead>
				<tbody>
					<tr v-for="data in teacherDetailCurriculum">
						<td v-text="data.courseSerial"></td>
						<td v-text="data.courseName"></td>
						<td v-text="data.courseSerialNumber"></td>
						<td v-text="data.courseTeacher"></td>
					</tr>
				</tbody>
			</table>
		</div>

		<Modal v-model="modalResult" id="modalBody" :styles="{top:'10rem'}">
			<div style="text-align:center; font-size:1.1rem;">
			    <p>请选择学期和周数！</p>
			</div>
		    <div slot="footer" style="text-align:center;">
		        <Button id="modalBtn" @click="resultOk()">确认</Button>
		    </div>
		</Modal>
	</div>
</div>
</template>

<script>
export default {
	data () {
		return {
			//year: '',
			// term: '',
			selSemester: '',
			yearSemester: [],
			selWeek: '',
			week: ["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20"],
			firstCourse: '',
			secondCourse: '',
			thirdCourse: '',
			fourthCourse: '',
			fifthCourse: '',
			sixthCourse: '',
			seventhCourse: '',
			eightCourse: '',
			ninthCourse: '',
			tenthCourse: '',
			eleventhCourse: '',
			twelfthCourse: '',
			thirteenthCourse: '',
			fourteenthCourse: '',
			classmeeting: '班会',
			teacherDetailCurriculum: [
			],
			modalResult: false
		}
	},
	beforeMount: function () {
		this.$http.post('./teacherSeeCurriculum',{},{
            "Content-Type":"application/json"
        }).then(function(response){
            var data = response.body;
            this.yearSemester = data.yearSemester;
            // this.week = data.week;
            this.firstCourse = data.teacherCurriculum[0].firstCourse;
            this.secondCourse = data.teacherCurriculum[0].secondCourse;
            this.thirdCourse = data.teacherCurriculum[0].thirdCourse;
            this.fourthCourse = data.teacherCurriculum[0].fourthCourse;
            this.fifthCourse = data.teacherCurriculum[0].fifthCourse;
            this.sixthCourse = data.teacherCurriculum[0].sixthCourse;
            this.seventhCourse = data.teacherCurriculum[0].seventhCourse;
            this.eightCourse = data.teacherCurriculum[0].eighthCourse;
            this.ninthCourse = data.teacherCurriculum[0].ninthCourse;
            this.tenthCourse = data.teacherCurriculum[0].tenthCourse;
            this.eleventhCourse = data.teacherCurriculum[0].eleventhCourse;
            this.twelfthCourse = data.teacherCurriculum[0].twelfthCourse;
            this.thirteenthCourse = data.teacherCurriculum[0].thirteenthCourse;
            this.fourteenthCourse = data.teacherCurriculum[0].fourteenthCourse;
            this.teacherDetailCurriculum = data.teacherDetailCurriculum;
            this.$http.post('./getTeachScoreList',{},{
              "Content-Type":"application/json"
            }).then(function(response){
              var data = response.body;
              this.selSemester = data.currentSemester;
            },function(error){});

        },function(error){
        });

    this.$http.post('./alternateLessionApplication',{},{
      "Content-Type":"application/json"
    }).then(function(response){
      var data = response.body;
      this.selWeek = data.week;
    },function(error){});

	},
	methods: {
		// 查询课表
		checkTableBtn: function () {
			if (this.selSemester != '' && this.selWeek != '') {
				this.$http.post('./teacherSeeCurriculum',{
		        	"yearSemester": this.selSemester,
		        	"week": this.selWeek
		        },{
		            "Content-Type":"application/json"
		        }).then(function(response){
		            var data = response.body;
		            this.firstCourse = data.teacherCurriculum[0].firstCourse;
		            this.secondCourse = data.teacherCurriculum[0].secondCourse;
		            this.thirdCourse = data.teacherCurriculum[0].thirdCourse;
		            this.fourthCourse = data.teacherCurriculum[0].fourthCourse;
		            this.fifthCourse = data.teacherCurriculum[0].fifthCourse;
		            this.sixthCourse = data.teacherCurriculum[0].sixthCourse;
		            this.seventhCourse = data.teacherCurriculum[0].seventhCourse;
		            this.eightCourse = data.teacherCurriculum[0].eighthCourse;
		            this.ninthCourse = data.teacherCurriculum[0].ninthCourse;
		            this.tenthCourse = data.teacherCurriculum[0].tenthCourse;
		            this.eleventhCourse = data.teacherCurriculum[0].eleventhCourse;
		            this.twelfthCourse = data.teacherCurriculum[0].twelfthCourse;
		            this.thirteenthCourse = data.teacherCurriculum[0].thirteenthCourse;
		            this.fourteenthCourse = data.teacherCurriculum[0].fourteenthCourse;
		            this.teacherDetailCurriculum = data.teacherDetailCurriculum;
		        },function(error){
		            console.log("获取申请error:");
		            console.log(error);
	        	});
			}else {
				this.modalResult = true;

			}
			// var tableInfo = document.getElementById('tableInfo');
			// tableInfo.action = 'index.php';
			// tableInfo.method = 'get';
		},
    	resultOk: function () {
    		this.modalResult = false;
    	}
	}
}
</script>

<style scoped>
#timetable {
	background-color: #f3f3f3;
	width: 100%;
	text-align: center;
}
.timetableBody,
.timetableList {
	padding: 1rem 5rem;
}
.timetableList {
	padding-bottom: 3.3rem;
}

</style>
