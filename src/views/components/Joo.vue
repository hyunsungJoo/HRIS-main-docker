<template>
      <div class="col-lg-7 mb-lg-0 mb-4">
        <div class="card">
          <div class="p-3 pb-0 card-header">
            <div class="d-flex justify-content-between">
              <h6 class="mb-2">부서별 급여</h6>
            </div>
          </div>
          <div class="table-responsive">
            <table class="table align-items-center">
              <thead>
                <tr>
                  <th>부서</th>
                  <th>급여</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(department, index) in departments" :key="index">
                  <td>{{ department.name }}</td>
                  <td>{{ department.salary }}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
      <div class="col-lg-5">
      </div>
  </template>

