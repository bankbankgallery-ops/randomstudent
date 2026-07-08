import React, { useState, useEffect, useRef } from 'react';
import { createClient } from '@supabase/supabase-js';
import { Trophy, UserPlus, Trash2, Settings, RefreshCw, Users, AlertCircle, Shuffle, Upload, CheckCircle2, ListPlus } from 'lucide-react';

// Initialize Supabase Client
const SUPABASE_URL = 'https://hkltkwxybevzratoomkf.supabase.co';
const SUPABASE_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImhrbHRrd3h5YmV2enJhdG9vbWtmIiwicm9sZSI6ImFub24iLCJpYXQiOjE3ODM0NzMzMTUsImV4cCI6MjA5OTA0OTMxNX0.QdLxoutOQB2bCgR6kdaz65nONUz24CI9v6ITb0amMQ8';
const supabase = createClient(SUPABASE_URL, SUPABASE_KEY);

export default function App() {
  // Data State
  const [students, setStudents] = useState([]);
  
  // App Logic State
  const [activeTab, setActiveTab] = useState('random'); // 'random' or 'manage'
  const [selectedClass, setSelectedClass] = useState('');
  const [allowDuplicates, setAllowDuplicates] = useState(false);
  const [pickedIds, setPickedIds] = useState([]); // IDs of students already picked (if duplicates not allowed)
  
  // Randomizer State
  const [isSpinning, setIsSpinning] = useState(false);
  const [currentResult, setCurrentResult] = useState(null);
  const [spinName, setSpinName] = useState('');
  
  // Form State for adding a new student
  const [newStudent, setNewStudent] = useState({
    prefix: 'เด็กชาย',
    firstName: '',
    lastName: '',
    classLevel: ''
  });

  // UI State
  const [errorMsg, setErrorMsg] = useState('');
  const [successMsg, setSuccessMsg] = useState('');
  const fileInputRef = useRef(null);

  const fetchStudents = async () => {
    try {
      const { data, error } = await supabase
        .from('students')
        .select('*')
        .order('classLevel', { ascending: true })
        .order('firstName', { ascending: true });
        
      if (error) throw error;
      
      setStudents(data || []);
      if (!selectedClass && data && data.length > 0) {
        const uniqueClasses = [...new Set(data.map(s => s.classLevel))].sort();
        if (uniqueClasses.length > 0) setSelectedClass(uniqueClasses[0]);
      }
    } catch (error) {
      console.error("Database error:", error);
      setErrorMsg("ไม่สามารถดึงข้อมูลรายชื่อได้ โปรดตรวจสอบการตั้งค่า Supabase");
    }
  };

  useEffect(() => {
    fetchStudents();
    
    // Setup real-time subscription for Supabase
    const channel = supabase
      .channel('public:students')
      .on('postgres_changes', { event: '*', schema: 'public', table: 'students' }, (payload) => {
        fetchStudents(); // Refresh data on any change
      })
      .subscribe();

    return () => {
      supabase.removeChannel(channel);
    };
  }, []); // Run once on mount

  const uniqueClasses = [...new Set(students.map(s => s.classLevel))].sort();

  const handleRandomize = () => {
    if (students.length === 0) {
      setErrorMsg('ยังไม่มีข้อมูลนักเรียนในระบบ');
      return;
    }

    // Filter students by selected class
    let eligibleStudents = students.filter(s => s.classLevel === selectedClass);
    
    if (eligibleStudents.length === 0) {
      setErrorMsg(`ไม่มีข้อมูลนักเรียนในชั้น ${selectedClass}`);
      return;
    }

    // Filter out already picked students if duplicates are not allowed
    if (!allowDuplicates) {
      eligibleStudents = eligibleStudents.filter(s => !pickedIds.includes(s.id));
    }

    if (eligibleStudents.length === 0) {
      setErrorMsg(`นักเรียนในชั้น ${selectedClass} ถูกสุ่มครบทุกคนแล้ว กรุณาล้างประวัติการสุ่มหรือเปิดโหมดสุ่มซ้ำ`);
      return;
    }

    setErrorMsg('');
    setCurrentResult(null);
    setIsSpinning(true);

    let spinCount = 0;
    const maxSpins = 20;
    const spinInterval = setInterval(() => {
      // Show random names rapidly
      const randomIdx = Math.floor(Math.random() * eligibleStudents.length);
      const tempStudent = eligibleStudents[randomIdx];
      setSpinName(`${tempStudent.prefix} ${tempStudent.firstName} ${tempStudent.lastName}`);
      
      spinCount++;
      if (spinCount >= maxSpins) {
        clearInterval(spinInterval);
        setIsSpinning(false);
        
        // Final selection
        const finalIdx = Math.floor(Math.random() * eligibleStudents.length);
        const winner = eligibleStudents[finalIdx];
        
        setCurrentResult(winner);
        setSpinName('');
        
        // Record picked ID
        if (!allowDuplicates) {
          setPickedIds(prev => [...prev, winner.id]);
        }
      }
    }, 100);
  };

  const handleAddStudent = async (e) => {
    e.preventDefault();
    if (!newStudent.firstName || !newStudent.lastName || !newStudent.classLevel) {
      setErrorMsg('กรุณากรอกข้อมูลให้ครบถ้วน');
      setTimeout(() => setErrorMsg(''), 3000);
      return;
    }

    try {
      const { error } = await supabase
        .from('students')
        .insert([{
          prefix: newStudent.prefix,
          firstName: newStudent.firstName,
          lastName: newStudent.lastName,
          classLevel: newStudent.classLevel
        }]);
        
      if (error) throw error;
      
      // Reset form fields but keep class level for convenience
      setNewStudent(prev => ({ ...prev, firstName: '', lastName: '' }));
      setErrorMsg('');
      setSuccessMsg('เพิ่มรายชื่อสำเร็จ');
      setTimeout(() => setSuccessMsg(''), 3000);
      if (!selectedClass) setSelectedClass(newStudent.classLevel);
    } catch (error) {
      console.error("Error adding student:", error);
      setErrorMsg('เกิดข้อผิดพลาดในการเพิ่มรายชื่อ: ' + error.message);
    }
  };

  const handleDeleteStudent = async (id) => {
    try {
      const { error } = await supabase
        .from('students')
        .delete()
        .eq('id', id);
        
      if (error) throw error;
      
      // Remove from picked history if it was there
      setPickedIds(prev => prev.filter(pid => pid !== id));
    } catch (error) {
      console.error("Error deleting student:", error);
      setErrorMsg('เกิดข้อผิดพลาดในการลบรายชื่อ');
    }
  };

  const resetPickedHistory = () => {
    setPickedIds([]);
    setCurrentResult(null);
    setSuccessMsg('ล้างประวัติการสุ่มเรียบร้อยแล้ว');
    setTimeout(() => setSuccessMsg(''), 3000);
  };

  const handleFileUpload = (e) => {
    const file = e.target.files[0];
    if (!file) return;

    const reader = new FileReader();
    reader.onload = async (event) => {
      const csvData = event.target.result;
      // Split by new line, handle different line endings
      const rows = csvData.split(/\r?\n/).filter(row => row.trim().length > 0);
      
      if (rows.length === 0) {
        setErrorMsg('ไฟล์ว่างเปล่า');
        return;
      }

      let startIndex = 0;
      // ตรวจสอบว่ามี Header ในแถวแรกหรือไม่ (เช็คคำที่มักใช้เป็นหัวตาราง)
      if (rows[0].includes('ชื่อ') || rows[0].includes('คำนำหน้า') || rows[0].includes('prefix') || rows[0].includes('name')) {
        startIndex = 1;
      }

      setSuccessMsg('กำลังนำเข้าข้อมูล...');
      
      const studentsToInsert = [];
      for (let i = startIndex; i < rows.length; i++) {
        // แยกข้อมูลด้วยลูกน้ำ (,)
        const cols = rows[i].split(',').map(col => col.trim());
        if (cols.length >= 4) {
          studentsToInsert.push({
            prefix: cols[0],
            firstName: cols[1],
            lastName: cols[2],
            classLevel: cols[3]
          });
        }
      }
      
      if (studentsToInsert.length > 0) {
        try {
          const { error } = await supabase
            .from('students')
            .insert(studentsToInsert);
            
          if (error) throw error;
          
          setSuccessMsg(`นำเข้าข้อมูลสำเร็จ ${studentsToInsert.length} รายการจากไฟล์`);
        } catch (error) {
          console.error("Error bulk adding rows", error);
          setErrorMsg('เกิดข้อผิดพลาดในการนำเข้าข้อมูล: ' + error.message);
        }
      } else {
         setErrorMsg('ไม่พบข้อมูลที่ถูกต้องในไฟล์ หรือรูปแบบคอลัมน์ไม่ถูกต้อง (ต้องการ 4 คอลัมน์)');
      }
      
      setTimeout(() => {
        setSuccessMsg('');
        setErrorMsg('');
      }, 5000);
      if (fileInputRef.current) fileInputRef.current.value = '';
    };
    reader.onerror = () => {
      setErrorMsg('ไม่สามารถอ่านไฟล์ได้');
    };
    reader.readAsText(file);
  };

  // Get counts for dashboard
  const totalStudents = students.length;
  const currentClassTotal = students.filter(s => s.classLevel === selectedClass).length;
  const pickedInClass = pickedIds.filter(id => students.find(s => s.id === id)?.classLevel === selectedClass).length;
  
  return (
    <div className="min-h-screen bg-slate-50 text-slate-900 font-sans">
      {/* Header */}
      <header className="bg-indigo-600 text-white p-6 shadow-md">
        <div className="max-w-4xl mx-auto flex flex-col sm:flex-row justify-between items-center gap-4">
          <div className="flex items-center gap-3">
            <Shuffle size={32} className="text-indigo-200" />
            <h1 className="text-2xl font-bold tracking-tight">ระบบสุ่มชื่อนักเรียน</h1>
          </div>
          
          <div className="flex bg-indigo-700/50 p-1 rounded-lg">
            <button 
              onClick={() => setActiveTab('random')}
              className={`px-4 py-2 rounded-md transition-all flex items-center gap-2 ${activeTab === 'random' ? 'bg-white text-indigo-700 shadow-sm' : 'text-indigo-100 hover:text-white'}`}
            >
              <Trophy size={18} />
              สุ่มชื่อ
            </button>
            <button 
              onClick={() => setActiveTab('manage')}
              className={`px-4 py-2 rounded-md transition-all flex items-center gap-2 ${activeTab === 'manage' ? 'bg-white text-indigo-700 shadow-sm' : 'text-indigo-100 hover:text-white'}`}
            >
              <Users size={18} />
              จัดการข้อมูล
            </button>
          </div>
        </div>
      </header>

      {/* Main Content Area */}
      <main className="max-w-4xl mx-auto p-4 md:p-6 mt-4">
        
        {/* Global Notifications */}
        {errorMsg && (
          <div className="mb-6 p-4 bg-red-50 border-l-4 border-red-500 text-red-700 flex items-center gap-3 rounded-md shadow-sm animate-pulse">
            <AlertCircle size={20} />
            <p>{errorMsg}</p>
          </div>
        )}
        
        {successMsg && (
          <div className="mb-6 p-4 bg-emerald-50 border-l-4 border-emerald-500 text-emerald-700 flex items-center gap-3 rounded-md shadow-sm">
            <CheckCircle2 size={20} />
            <p>{successMsg}</p>
          </div>
        )}

        {}
        {activeTab === 'random' && (
          <div className="space-y-6">
            
            {/* Control Panel */}
            <div className="bg-white p-6 rounded-2xl shadow-sm border border-slate-100">
              <div className="flex flex-col md:flex-row gap-6 justify-between">
                
                <div className="flex-1 space-y-2">
                  <label className="block text-sm font-semibold text-slate-600">เลือกระดับชั้นที่ต้องการสุ่ม</label>
                  <select 
                    value={selectedClass} 
                    onChange={(e) => {
                      setSelectedClass(e.target.value);
                      setCurrentResult(null);
                    }}
                    className="w-full md:w-64 p-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 transition-all outline-none"
                  >
                    {uniqueClasses.length === 0 && <option value="">-- ไม่มีข้อมูล --</option>}
                    {uniqueClasses.map(c => (
                      <option key={c} value={c}>{c}</option>
                    ))}
                  </select>
                </div>

                <div className="flex flex-col gap-3">
                  <label className="flex items-center gap-3 cursor-pointer p-3 bg-slate-50 rounded-xl border border-slate-200 hover:bg-slate-100 transition-colors">
                    <div className="relative">
                      <input 
                        type="checkbox" 
                        className="sr-only" 
                        checked={allowDuplicates}
                        onChange={(e) => setAllowDuplicates(e.target.checked)}
                      />
                      <div className={`block w-10 h-6 rounded-full transition-colors ${allowDuplicates ? 'bg-indigo-500' : 'bg-slate-300'}`}></div>
                      <div className={`dot absolute left-1 top-1 bg-white w-4 h-4 rounded-full transition-transform ${allowDuplicates ? 'transform translate-x-4' : ''}`}></div>
                    </div>
                    <span className="text-sm font-medium text-slate-700">อนุญาตให้สุ่มซ้ำคนเดิมได้</span>
                  </label>
                  
                  {!allowDuplicates && (
                     <div className="flex justify-between items-center text-xs text-slate-500 px-1">
                       <span>สุ่มแล้ว: {pickedInClass}/{currentClassTotal} คน</span>
                       <button onClick={resetPickedHistory} className="text-indigo-600 hover:underline flex items-center gap-1">
                         <RefreshCw size={12} /> ล้างประวัติ
                       </button>
                     </div>
                  )}
                </div>
              </div>
            </div>

            {/* Display Area */}
            <div className="bg-white rounded-3xl shadow-lg border border-slate-200 overflow-hidden min-h-[400px] flex flex-col">
              
              <div className="flex-1 flex flex-col items-center justify-center p-8 relative">
                {/* Background Decoration */}
                <div className="absolute inset-0 bg-gradient-to-br from-indigo-50 to-blue-50 opacity-50 z-0"></div>
                
                <div className="z-10 text-center w-full">
                  {!isSpinning && !currentResult && (
                    <div className="text-slate-400 flex flex-col items-center gap-4">
                      <Trophy size={64} className="text-slate-200" />
                      <p className="text-xl">กดปุ่มด้านล่างเพื่อเริ่มสุ่มผู้โชคดี!</p>
                      {selectedClass && <p className="text-sm">เป้าหมาย: ชั้น {selectedClass}</p>}
                    </div>
                  )}

                  {isSpinning && (
                    <div className="animate-pulse">
                      <p className="text-slate-500 mb-2 font-medium">กำลังสุ่มค้นหา...</p>
                      <h2 className="text-4xl md:text-5xl lg:text-6xl font-bold text-indigo-600 tracking-tight">
                        {spinName}
                      </h2>
                    </div>
                  )}

                  {currentResult && !isSpinning && (
                    <div className="animate-in zoom-in duration-300">
                      <div className="inline-block px-4 py-1 bg-emerald-100 text-emerald-700 rounded-full text-sm font-bold mb-4">
                        ผู้โชคดีคือ
                      </div>
                      <h2 className="text-4xl md:text-5xl lg:text-6xl font-bold text-slate-800 tracking-tight mb-4 drop-shadow-sm">
                        <span className="text-indigo-600">{currentResult.prefix}</span> {currentResult.firstName} {currentResult.lastName}
                      </h2>
                      <p className="text-xl text-slate-500 font-medium">
                        ชั้น {currentResult.classLevel}
                      </p>
                    </div>
                  )}
                </div>
              </div>

              {/* Action Button */}
              <div className="p-6 bg-slate-50 border-t border-slate-100 flex justify-center">
                <button
                  onClick={handleRandomize}
                  disabled={isSpinning || students.length === 0}
                  className={`
                    relative group overflow-hidden rounded-2xl px-12 py-5 font-bold text-xl text-white shadow-lg transition-all
                    ${isSpinning || students.length === 0 ? 'bg-slate-400 cursor-not-allowed shadow-none' : 'bg-indigo-600 hover:bg-indigo-700 hover:scale-105 active:scale-95 shadow-indigo-600/30 hover:shadow-indigo-600/50'}
                  `}
                >
                  <span className="relative z-10 flex items-center gap-3">
                    {isSpinning ? <RefreshCw className="animate-spin" size={24} /> : <Shuffle size={24} />}
                    {isSpinning ? 'กำลังสุ่ม...' : 'เริ่มสุ่มรายชื่อ!'}
                  </span>
                  {/* Button shine effect */}
                  <div className="absolute inset-0 h-full w-full bg-gradient-to-r from-transparent via-white/20 to-transparent -translate-x-full group-hover:animate-[shimmer_1.5s_infinite] transition-all"></div>
                </button>
              </div>
            </div>
          </div>
        )}

        {}
        {activeTab === 'manage' && (
          <div className="space-y-6">
            
            <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
              
              {/* Add Student Form */}
              <div className="md:col-span-1 space-y-6">
                
                <div className="bg-white p-5 rounded-2xl shadow-sm border border-slate-200">
                  <h3 className="text-lg font-bold flex items-center gap-2 mb-4 text-slate-800">
                    <UserPlus size={20} className="text-indigo-600" />
                    เพิ่มรายชื่อใหม่
                  </h3>
                  <form onSubmit={handleAddStudent} className="space-y-4">
                    <div>
                      <label className="block text-xs font-semibold text-slate-500 uppercase tracking-wider mb-1">คำนำหน้า</label>
                      <select 
                        value={newStudent.prefix}
                        onChange={(e) => setNewStudent({...newStudent, prefix: e.target.value})}
                        className="w-full p-2.5 bg-slate-50 border border-slate-200 rounded-lg focus:ring-2 focus:ring-indigo-500 outline-none"
                      >
                        <option value="เด็กชาย">เด็กชาย</option>
                        <option value="เด็กหญิง">เด็กหญิง</option>
                        <option value="นาย">นาย</option>
                        <option value="นางสาว">นางสาว</option>
                      </select>
                    </div>
                    <div>
                      <label className="block text-xs font-semibold text-slate-500 uppercase tracking-wider mb-1">ชื่อ</label>
                      <input 
                        type="text" 
                        placeholder="ชื่อจริง"
                        value={newStudent.firstName}
                        onChange={(e) => setNewStudent({...newStudent, firstName: e.target.value})}
                        className="w-full p-2.5 bg-slate-50 border border-slate-200 rounded-lg focus:ring-2 focus:ring-indigo-500 outline-none"
                      />
                    </div>
                    <div>
                      <label className="block text-xs font-semibold text-slate-500 uppercase tracking-wider mb-1">นามสกุล</label>
                      <input 
                        type="text" 
                        placeholder="นามสกุล"
                        value={newStudent.lastName}
                        onChange={(e) => setNewStudent({...newStudent, lastName: e.target.value})}
                        className="w-full p-2.5 bg-slate-50 border border-slate-200 rounded-lg focus:ring-2 focus:ring-indigo-500 outline-none"
                      />
                    </div>
                    <div>
                      <label className="block text-xs font-semibold text-slate-500 uppercase tracking-wider mb-1">ระดับชั้น</label>
                      <input 
                        type="text" 
                        placeholder="เช่น ม.1/1, ป.6"
                        value={newStudent.classLevel}
                        onChange={(e) => setNewStudent({...newStudent, classLevel: e.target.value})}
                        className="w-full p-2.5 bg-slate-50 border border-slate-200 rounded-lg focus:ring-2 focus:ring-indigo-500 outline-none"
                      />
                    </div>
                    <button 
                      type="submit"
                      className="w-full py-3 bg-indigo-600 hover:bg-indigo-700 text-white rounded-lg font-semibold transition-colors flex items-center justify-center gap-2"
                    >
                      <ListPlus size={18} />
                      บันทึกรายชื่อ
                    </button>
                  </form>
                </div>

                {/* File Upload Section */}
                <div className="bg-slate-100 p-5 rounded-2xl border border-slate-200 border-dashed">
                  <h3 className="text-base font-bold flex items-center gap-2 mb-2 text-slate-700">
                    <Upload size={18} />
                    นำเข้าข้อมูลจาก CSV
                  </h3>
                  <p className="text-xs text-slate-500 mb-4 leading-relaxed">
                    อัปโหลดไฟล์ "รายชื่อนักเรียนทำโปรแกรมสุ่มชื่อ.csv" ของคุณ (รูปแบบ: คำนำหน้า,ชื่อ,สกุล,ชั้น)
                  </p>
                  <label className="cursor-pointer block w-full py-3 px-4 bg-white border border-slate-300 rounded-lg text-center text-sm font-medium text-slate-600 hover:bg-slate-50 hover:text-indigo-600 hover:border-indigo-300 transition-colors">
                    เลือกไฟล์ CSV
                    <input 
                      type="file" 
                      accept=".csv" 
                      className="hidden" 
                      ref={fileInputRef}
                      onChange={handleFileUpload}
                    />
                  </label>
                </div>

              </div>

              {/* Data Table */}
              <div className="md:col-span-2 bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden flex flex-col h-[600px]">
                <div className="p-4 border-b border-slate-100 flex justify-between items-center bg-slate-50">
                  <h3 className="font-bold flex items-center gap-2 text-slate-800">
                    <Users size={18} className="text-indigo-600" />
                    รายชื่อนักเรียนทั้งหมด ({totalStudents} คน)
                  </h3>
                  <div className="flex items-center gap-2">
                    <span className="text-xs font-semibold text-slate-500">กรองชั้น:</span>
                    <select 
                      value={selectedClass}
                      onChange={(e) => setSelectedClass(e.target.value)}
                      className="p-1.5 text-sm bg-white border border-slate-200 rounded-md outline-none focus:ring-1 focus:ring-indigo-500"
                    >
                      <option value="">ทั้งหมด</option>
                      {uniqueClasses.map(c => (
                         <option key={c} value={c}>{c}</option>
                      ))}
                    </select>
                  </div>
                </div>
                
                <div className="flex-1 overflow-y-auto p-0">
                  {students.length === 0 ? (
                    <div className="h-full flex flex-col items-center justify-center text-slate-400 p-8 text-center">
                      <Users size={48} className="mb-4 opacity-20" />
                      <p>ยังไม่มีข้อมูลนักเรียน</p>
                      <p className="text-sm mt-2">กรุณาเพิ่มข้อมูลรายบุคคล หรือนำเข้าไฟล์ CSV</p>
                    </div>
                  ) : (
                    <table className="w-full text-sm text-left">
                      <thead className="text-xs text-slate-500 uppercase bg-slate-50 sticky top-0 z-10 shadow-sm">
                        <tr>
                          <th className="px-6 py-3 font-semibold">ชื่อ - สกุล</th>
                          <th className="px-6 py-3 font-semibold w-24">ระดับชั้น</th>
                          <th className="px-6 py-3 font-semibold text-right w-24">จัดการ</th>
                        </tr>
                      </thead>
                      <tbody className="divide-y divide-slate-100">
                        {students
                          .filter(s => selectedClass === "" || s.classLevel === selectedClass)
                          .map((student) => (
                          <tr key={student.id} className="hover:bg-slate-50 transition-colors">
                            <td className="px-6 py-3">
                              <span className="text-slate-500 mr-1">{student.prefix}</span>
                              <span className="font-medium text-slate-800">{student.firstName} {student.lastName}</span>
                            </td>
                            <td className="px-6 py-3">
                              <span className="inline-block px-2 py-1 bg-indigo-50 text-indigo-700 rounded-md text-xs font-semibold">
                                {student.classLevel}
                              </span>
                            </td>
                            <td className="px-6 py-3 text-right">
                              <button 
                                onClick={() => handleDeleteStudent(student.id)}
                                className="text-slate-400 hover:text-red-500 transition-colors p-1"
                                title="ลบรายชื่อนี้"
                              >
                                <Trash2 size={16} />
                              </button>
                            </td>
                          </tr>
                        ))}
                      </tbody>
                    </table>
                  )}
                </div>
              </div>
            </div>
            
          </div>
        )}
      </main>
      
      {/* Footer */}
      <footer className="max-w-4xl mx-auto mt-12 pb-6 text-center text-sm text-slate-400">
        <p>เชื่อมต่อกับฐานข้อมูล Supabase • พร้อมใช้งาน</p>
      </footer>
    </div>
  );
}
